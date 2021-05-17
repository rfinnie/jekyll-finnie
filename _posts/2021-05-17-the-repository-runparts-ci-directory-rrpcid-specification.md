---
date: 2021-05-17 16:21:17-07:00
excerpt: 'Wherein Ryan doesn''t realize xkcd #927 was meant to be a cautionary tale.'
excerpt_standalone: true
layout: post
title: The Repository Run-Parts CI Directory (RRPCID) specification
---
Years ago, I wrote [dsari](https://github.com/rfinnie/dsari) ("Do Something And Record It"), a lightweight CI system. This was prompted by administering Jenkins installations for multiple development groups at the time, each environment having increasingly specialized (and often incompatible) plugins layered onto the core functionality.

This led me to take the opposite approach.  I made a CI system based on one executable (usually a script) per job, and the assumption that you, the CI job developer, know exactly what functionality you want.  Want custom notifications?  [Write it into the script.](https://github.com/rfinnie/dsari/blob/main/doc/notifications.md)  Sub-job triggers based on the result of the run?  [You can totally do that.](https://github.com/rfinnie/dsari/blob/main/doc/triggers.md)  Remote agents?  Bah, just tell the script to ssh to a remote system [based on the concurrency group the run is currently in](https://github.com/rfinnie/dsari/blob/main/doc/concurrency.md).  dsari's acronym was a light-hearted take on this simplicity.

Fast forward to now, and GitHub's CI has quickly become ubiquitous.  But before that, Travis essentially pushed the idea of in-repository CI definitions, as opposed to a CI job being built around the repository as in Jenkins.  As an example, [finnix-live-build](https://github.com/finnix/finnix-live-build) has a GitHub workflow which makes a test build, but I also have multiple dsari instances at home, for different architectures, doing the same thing on schedule.

However, the dsari job script merely replicates the build process of the GitHub workflow.  If I add new functionality to the GitHub workflow, I need to also update build scripts on 5 different machines.  This would be great to move in-repository, but I quickly found there is no established general-purpose in-repository CI layout.

So let's make one!

If the closest simplification to the Jenkins CI model is `cron`, the closest simplification to the GitHub CI model is `run-parts`.  However, since `run-parts` has different functionality on different systems (with Debian's implementation currently being the most versatile), the "Run-Parts" part of the RRPCID acronym is in spirit only (though you could use Debian's `run-parts --exit-on-error` for the job processing part of the RRPCID logic).

Here's the specification I came up with:

  * A workflow is a collection of jobs, and is a readable directory under `.rrpcid/workflows/`.
  * A job is a collection of actions, and is a readable directory under `${workflow_dir}/jobs/`.
  * An action is an executable file under `${job_dir}/`.  In theory this can be anything, but is likely to be a shell script.
  * Actions are executed with the repository as the current working directory.
  * Actions are executed with the environment variable CI=true. Other environment variables may be passed in from the underlying CI manager.
  * Actions are executed in lexical sort order within the job directory.
  * Workflow, job and action names must only contain letters a through z and A through Z, numbers 0 through 9, and characters "-" (dash) and "_" (underscore).  Note specifically the lack of "." (period).
  * A repository may have multiple workflows, a workflow may have multiple jobs, and a job may have multiple actions.
  * If an action exits with a status other than 0, further actions in a job are skipped.
  * All jobs in a workflow are run, regardless of whether other jobs' actions have failed.
  * Actions within the job must not assume another job has previously run.
  * An implicit, unnamed workflow lives directly in `.rrpcid/`.
  * Whether all, some or none of the repository's workflows are run is up to the CI manager; that logic is outside the scope of this specification.
  * All other files and directories are ignored. For example, a directory named `.rrpcid/testdata/` is outside the scope of this specification, and would not be handled.
  * A recommended directory for generated artifacts is `artifacts/` within the workflow directory, but a CI manager is not required to do anything with this.

A script layout utilizing multiple workflows (including the implicit unnamed workflow) and multiple jobs might look like this:

```
.rrpcid/jobs/ci/action_1
.rrpcid/jobs/ci/action_2
.rrpcid/jobs/lint/run-lint
.rrpcid/workflows/deploy/jobs/env1/deploy
.rrpcid/workflows/deploy/jobs/env2/deploy
.rrpcid/workflows/deploy/jobs/archive/01tar
.rrpcid/workflows/deploy/jobs/archive/02upload
```

The following shell code would satisfy the above requirements, assuming it's being run from dash/bash (both sort "*" matches which are needed for the job directory; other Bourne shells may not).  It satisfies the requirements, but is by no means the only way to implement an RRPCID processor.

```shell
export CI=true
run_workflow() {
    for job_dir in "${1}/jobs"/*; do
        [ -d "${job_dir}" ] || continue
        [ -x "${job_dir}" ] || continue
        [ -z "$(basename "${job_dir}" | sed 's/[a-zA-Z0-9_-]//g')" ] || continue
        for action in "${job_dir}"/*; do
            [ -x "${action}" ] || continue
            [ -z "$(basename "${action}" | sed 's/[a-zA-Z0-9_-]//g')" ] || continue
            "${action}" || break
        done
    done
}

run_workflow .rrpcid
for workflow_dir in .rrpcid/workflows/*; do
    [ -d "${workflow_dir}" ] || continue
    [ -x "${workflow_dir}" ] || continue
    [ -z "$(basename "${workflow_dir}" | sed 's/[a-zA-Z0-9_-]//g')" ] || continue
    run_workflow "${workflow_dir}"
done
```

[finnix-live-build now has a simple RRPCID job](https://github.com/finnix/finnix-live-build/tree/main/.rrpcid), though as of this writing I have not yet switched over the home dsari jobs to utilize it.

---

Side note / rant: I went back and forth on whether to allow "." as part of the names, specifically the action script names.  Historically, ignoring "." has been traditional for `run-parts`, `cron.d`, etc, because it ignores automatically-created files such as `foo.bak`, `foo.swp`, etc.  However, I acknowledge using extensions for executable scripts within a project (`.sh`, `.py`, etc) is currently popular.

My answer to this? Stop Doing That. I can't tell you how many times I've seen someone (including me) put `do_cool_thing.sh` into a repository, and over time it gets expanded to the point it's too complicated to be effectively managed as a shell script, and is rewritten in, say, Python.  The problem is references to `do_cool_thing.sh` are now too entrenched, and you now have `do_cool_thing.sh` which is actually a Python script (!!!), or `do_cool_thing.sh` which is just a wrapper call to `do_cool_thing` (if I learned my lesson) or `do_cool_thing.py` (if I didn't).

Just drop the extension when creating a script.  Shebangs exist for a reason. 😉

By the way, if you do find yourself in this migration situation, here's a general-purpose redirect script for the old location:

```shell
#!/bin/sh

exec "$(dirname "$0")/$(basename "$0" .sh)" "$@"
```
