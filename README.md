# TARS-lunar-jumping-robot
Software for TARS, an autonomous jumping robot built for NASA HUNCH to scout lunar terrain — ESP32-CAM video streaming, motor control, and autonomous launch sequencing in C++.
# TARS — Autonomous Lunar Terrain-Scouting Robot

## Overview
TARS is an autonomous jumping robot built for NASA HUNCH's Design and Prototype 
program during the 2025–2026 school year, as part of NASA's Artemis-era Moon to 
Mars initiative. Astronauts on the lunar surface will need higher-resolution 
terrain data than the Lunar Reconnaissance Orbiter can currently provide (0.5m/pixel). 
TARS was designed to be deployed by astronauts to scout the surrounding terrain 
via a spring-loaded jump mechanism and an onboard camera, gathering close-up 
imagery to help identify areas of interest for surface missions.

Built by a 4-person team at Green Mountain High School, TARS was selected as one 
of 8–10 national finalist teams and invited to Johnson Space Center in Houston, TX, 
to present to NASA astronauts and engineers. Of the finalist teams, only 4 — 
including ours — successfully got their robot to jump autonomously, and multiple 
times in a row.

## My Role
I was the software lead for TARS, responsible for the onboard control system 
running on an ESP32-CAM.

## What the Code Does
The ESP32-CAM video streaming and web server code was adapted from a Random 
Nerd Tutorials open-source project, which I modified and integrated into the 
rest of the robot's control system, rather than writing the camera/streaming 
logic from scratch.

- **Live video streaming**: Streams real-time video from the onboard camera over 
  Wi-Fi to a browser interface, giving operators visual eyes on the terrain 
  without needing to be physically present. Built on a Random Nerd Tutorials 
  ESP32-CAM base, customized to fit TARS's control system.
- **Motor control**: Drives the spring-release mechanism via H-bridge motor 
  control to trigger each jump. Written from scratch for TARS.
- **Autonomous launch sequencing**: Runs a timed loop that fires repeated jumps 
  with no operator input in between, with LED status indicators showing the 
  robot's current state (idle, arming, launching). Written from scratch for TARS.

## The Hard Part: From Button to Hardcoded Sequence
The original design used a physical button to manually trigger each jump. When the
motor reached the bottom of the platform, it would signal to release the mechanism
and cause a fully autonomous jump without a hard coded sequence. Under 
motor load, the system started brownout-resetting — the current draw from the 
motor was dropping the voltage enough to reset the ESP32 mid-launch, which meant 
the button-triggered approach was unreliable right when it mattered most.

With the competition deadline approaching, I made the call to abandon the 
button entirely and rewrite the launch logic as a hardcoded, pre-programmed 
timing sequence instead. Rather than waiting on an external trigger that could 
fail under load, the robot would run its jumps on a fixed internal timer, 
removing the failure point altogether. It wasn't the most elegant fix, but it 
was the reliable one, and it's what got TARS to the four-team club of robots 
that could actually jump autonomously and repeatedly on demand.

## Tech Stack
- ESP32-CAM (C++ / Arduino framework)
- H-bridge motor control
- Addressable LEDs (NeoPixel) for status feedback
- Wi-Fi video streaming + browser-based control interface

## Outcome
- Selected as a NASA HUNCH national finalist (1 of 8–10 teams)
- One of only 4 finalist teams whose robot successfully jumped autonomously, 
  multiple times in a row
- Presented the working prototype and defended design decisions in NASA-style 
  design reviews at Johnson Space Center
