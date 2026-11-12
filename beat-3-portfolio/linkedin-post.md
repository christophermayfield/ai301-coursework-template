I opened a pull request to ArduPilot — a 16,000-star open-source drone autopilot used in real vehicles — to add support for a sensor data format the codebase was silently ignoring.

The issue: ArduPilot handled one type of optical flow data from companion computers but dropped a second type, `OPTICAL_FLOW_RAD`, without any error or warning. I traced the gap to three files in the codebase, wrote a handler that converts the incoming data to the format ArduPilot's existing code expects, and submitted a fix across five files in three subsystems.

The first version got rejected — not by a human, but by my own automated checking tool, which found that ArduPilot requires one subsystem per commit and I had combined three in one. I split the changes, the tool passed the second time, and the PR is now open and awaiting review. CI found a second bug after the PR opened: I'd defined a configuration macro in a file that isn't always included at the right time in the build chain. I fixed that too with two more commits.

What I took away: reading a large, safety-critical codebase by following one function call end-to-end is faster than trying to map the whole thing. And having a tool that checks your own work before it goes out is worth building.

https://github.com/ArduPilot/ardupilot/pull/35145

#OpenSource #EmbeddedSystems #DroneEngineering #MAVLink #AIEngineering
