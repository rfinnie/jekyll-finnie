---
author: Ryan Finnie
categories:
- Uncategorized
date: 2007-07-09 13:34:00
guid: http://www.finnie.org/2007/07/09/if-you-cant-take-the-heat-get-out-of-the-noc/
id: 688
layout: post
lj_import_url:
- http://fo0bar.livejournal.com/180930.html
lj_itemid:
- '180930'
permalink: /2007/07/09/if-you-cant-take-the-heat-get-out-of-the-noc/
title: If you can't take the heat, get out of the NOC.
---
_(I wrote this story to submit to The Daily WTF, so it's simplified in some places. Keep this in mind if you know details about the company and want to go "THAT'S NOT EXACTLY HOW IT HAPPENED!")_

"Failover Networks" was designed to be the best of the best in the datacenter world. High security, multiple upstream networks, dual datacenter design (one on the west coast, one on the east), all the luxuries.

Sadly, we learned the lesson that you probably shouldn't build datacenters targeted at high-end clients immediately after the dot-com bubble burst, and Failover Networks itself went out of business. At the west coast datacenter, a few employees were retained after the rest were laid off, to help the existing clients move out and turn off the lights. There was a business-type person, a security guard, and myself, to look after the network.

Two days after the layoffs, I received an SMS page, alerting me that a server had gone down in the MIS room (which held internal company servers, DNS nameservers, etc). The particular server was not critical during this "turn-down" period, so I didn't run to the MIS room to investigate. However, 30 minutes later, 2 more servers went down, so I went to the MIS room.

When I opened the door, a blast of hot air greeted me. The thermostat read over 110 degrees Fahrenheit. I ran to the shipping/receiving room, took the plastic wrap off the emergency portable A/C unit (which had never been needed or used during the datacenter's lifetime), quickly skimmed over the instructions, and wheeled it into the MIS room.

After a little trial-and-error (a quick lesson in thermodynamics: if cool air comes out one side, hot air must come out the other side; the collapsible accordion vent is used to route that hot air out of the room), the room was back down to a workable 85 degrees. With that safe for the moment, I went to the security guard, who also monitors the HVAC systems.

"That can't be!" he said. "The MIS room is 64 degrees, in a zone set for 68. The heater actually kicked in to bring it back up to 68." After assuring him it was just a bit warmer than 64 degrees in there, he manually overrode the zone, and we started discussing what could have happened. Faulty sensor, perhaps? What do the other zones say?

"Datacenter floor?" "Fine." "Node room?" "Fine." "Offices?" "Fine." "Lobby?" "Fine."

"NOC?" "... Uhh, I don't see a NOC zone."

After a brief tracing of cables and sensors, we pieced together what happened. The Network Operations Center room has been empty since the layoffs 2 days earlier. The NOC itself was mainly for show; I could do everything I needed to from my cubicle in the offices. As it turns out, when the building was built, the HVAC guys messed up and placed the NOC and MIS rooms in the same zone. The sensor was in the NOC room, and the vents were in the MIS room. Before the layoffs, the NOC was staffed 24/7, and the presence of human heat always kept the ambient temperature above 68F, thus keeping the A/C blowing all the time in the MIS room. With no humans in the NOC, the HVAC system started pumping heat into the MIS room.

The zone was "tweaked" to 50 degrees so it would once again start blowing cold air, and the failed servers eventually came back up. 2 weeks later I took my final paycheck and happily pulled the breaker to the MIS room, as the network was no longer needed.
