---
date: 2018-04-22 17:55:04-07:00
excerpt: In 2015, I released dsari (Do Something and Record It), a small but very
  powerful continuous integration (CI) system.  Today version 2.0 has been released.
layout: post
title: dsari (Do Something and Record It) CI 2.0 released
---
[In 2015](https://www.finnie.org/2015/07/23/dsari-do-something-and-record-it/), I released [dsari](https://github.com/rfinnie/dsari) (Do Something and Record It), a small but very powerful continuous integration (CI) system.  Over the past few years, I've been adding bits and pieces to it, and realized I haven't made a proper release since shortly after the initial release.

Version 2.0 includes the following:

* 90% refactor, and rewritten in Python 3.  Note when upgrading you will need to install Python 3 versions of dependent libraries (e.g. `apt-get install python3-croniter`).
* Configurable database support: SQLite (default), PostgreSQL, MySQL, or MongoDB.
* Added dsari-prometheus-exporter for exporting job/run metrics to [Prometheus](https://prometheus.io/).
* Added dsari-info command, allowing for CLI retrieval of various dsari job/run information.
* Added support for iCalendar RRULE schedule definitions (via python-dateutil), instead of or in addition to hashed cron format (via croniter).
* New job option: concurrent_runs - Allows multiple runs of the same job to run concurrently.
* HUP reloading is now immediate, rather than next time dsari-daemon is idle.
* Triggers may now request a time to run the trigger, instead of as soon as possible.
* Many minor fixes.

Most of these changes were done gradually over the last two years, but what got me on the latest development kick was actually a recent facepalm moment.  Recently I had the idea to take Nagios NRPE definitions, run them, and export the data (exit code, running length, etc) to Prometheus, rather than using Nagios directly.  But then I started thinking "I'll need to deal with tracking forks, concurrency, etc, etc.  That'll be a pain."

"Oh wait, I've already solved this.  This sounds like a situation where I need to Do Something and Record It(s metrics)."

With that, I wrote dsari-prometheus-exporter and integrated it with dsari.  The metrics it exports can be useful; see below for the metrics from a sample job.

The `sample1` job is part of a severely constrained concurrency group where only one job run in the group is configured to be running at once.  The actual run time is remarkably consistent at about 90 seconds, but since there are a number of jobs in this concurrency group and they all want to run every minute, there is always a waiting list.  As a result, this job runs about 70 seconds late on average.  But the spread is quite large, from (effectively) immediately, to nearly 6 minutes late.

(The fact that over 1300 runs, the job has spent almost exactly the same amount of time running as being blocked is also interesting, but a coincidence.)

```
# HELP dsari_run_latency_seconds Length of time spent between scheduled start and actual start
# TYPE dsari_run_latency_seconds summary
dsari_run_latency_seconds{job_name="sample1",quantile="0.01"} 0.0027895
dsari_run_latency_seconds{job_name="sample1",quantile="0.1"} 34.53752
dsari_run_latency_seconds{job_name="sample1",quantile="0.5"} 69.8487545
dsari_run_latency_seconds{job_name="sample1",quantile="0.9"} 170.2028667
dsari_run_latency_seconds{job_name="sample1",quantile="0.99"} 358.96789803
dsari_run_latency_seconds_count{job_name="sample1"} 1300
dsari_run_latency_seconds_sum{job_name="sample1"} 113835.42681500006

# HELP dsari_run_duration_seconds Length of time spent in a run
# TYPE dsari_run_duration_seconds summary
dsari_run_duration_seconds{job_name="sample1",quantile="0.01"} 5.06363971
dsari_run_duration_seconds{job_name="sample1",quantile="0.1"} 90.03290849999999
dsari_run_duration_seconds{job_name="sample1",quantile="0.5"} 90.060451
dsari_run_duration_seconds{job_name="sample1",quantile="0.9"} 90.0872659
dsari_run_duration_seconds{job_name="sample1",quantile="0.99"} 90.10349761
dsari_run_duration_seconds_count{job_name="sample1"} 1300
dsari_run_duration_seconds_sum{job_name="sample1"} 112009.71985500008

# HELP dsari_last_run_exit_code Numeric exit code of the last run for a job
# TYPE dsari_last_run_exit_code gauge
dsari_last_run_exit_code{job_name="o1"} 143

# HELP dsari_last_run_schedule_time Schedule time of the last run for a job, seconds since epoch
# TYPE dsari_last_run_schedule_time gauge
dsari_last_run_schedule_time{job_name="o1"} 1524347585.946814

# HELP dsari_last_run_start_time Start time of the last run for a job, seconds since epoch
# TYPE dsari_last_run_start_time gauge
dsari_last_run_start_time{job_name="o1"} 1524347824.892004

# HELP dsari_last_run_stop_time Stop time of the last run for a job, seconds since epoch
# TYPE dsari_last_run_stop_time gauge
dsari_last_run_stop_time{job_name="o1"} 1524347913.404784

# HELP dsari_run_count Number of runs performed for a job
# TYPE dsari_run_count counter
dsari_run_count{job_name="o1"} 1300

# HELP dsari_run_failure_count Number of failed runs performed for a job
# TYPE dsari_run_failure_count counter
dsari_run_failure_count{job_name="o1"} 56

# HELP dsari_run_success_count Number of successful runs performed for a job
# TYPE dsari_run_success_count counter
dsari_run_success_count{job_name="o1"} 1244
```