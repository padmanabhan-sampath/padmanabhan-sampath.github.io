---
layout: post
title: "Tracking the ISS with a CCTV camera pan-tilt mount: the first live run"
excerpt: "A cheap CCTV pan-tilt mount turned antenna rotator, tested on an ISS SSTV pass with Doppler-corrected reception."
date: 2026-10-11
tags: [ISS, SSTV, Satellites, Amateur Radio, ESP32]
---

<head>
  <link rel="stylesheet" href="/css/main.css">
</head>

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/setup.jpg" width="100%" alt="The pan-tilt mount with a 3-element Yagi on a tripod at the balcony, and a labelled close-up of the build" />
  <figcaption><em>Left: set up on the balcony for the pass. Right: the build, with the CCTV mount, the 3D-printed antenna bracket, the 9-DoF IMU and the ESP32.</em></figcaption>
</figure>

# **TL;DR**

>> - Built a simple satellite antenna rotator from a **CCTV pan-tilt mount**, an **ESP32** and a 3D-printed bracket.
>> - Demonstrated it on an **ISS SSTV** pass, with Doppler-corrected reception and decoded images.

# **Table of Contents**

1. [Introduction](#introduction)
2. [Setup](#setup)
3. [The Satellite Pass](#the-satellite-pass)
4. [Results](#results)
5. [Conclusion](#conclusion)

# **Introduction**

In my earlier posts ([minimal setup]({% post_url 2023-03-26-iss-sstv %}),
[Yagi-Uda antenna]({% post_url 2024-05-26-iss-sstv-yagi %})), I pointed the antenna at the ISS by hand. This time I automated it with a small, portable tracker built around a CCTV
pan-tilt mount.

This post covers the setup and the first live run, during the
[ARISS SSTV Series 33 "Student Education" event](https://issfanclub.eu/2026/09/24/ariss-sstv-event-starting-2-october-2026-900-utc-ending-6-october-2026-time-tbd/)
(437.550 MHz FM, Robot 36).

# **Setup**

#### **Hardware**

<!-- TODO: link and price for the SK-BK03MPT-232 mount -->

| # | Item | Function | Notes |
|---|---|---|---|
| 1 | Pelco-D CCTV pan-tilt mount (SK-BK03MPT-232) | Points the antenna | About 225° of pan and 50° of tilt. TTL UART version, no encoders |
| 2 | ESP32 (TTGO T-Display) | The bridge between Hamlib and the mount | Talks Pelco-D to the mount and Hamlib's rotator protocol over WiFi |
| 3 | 9-DOF IMU (LSM9DS1) | Feedback on the mount's movement and tilt | Accelerometer, gyro and magnetometer |
| 4 | UHF 3-element Yagi-Uda antenna | Receive antenna | About 60° wide beam, so the pointing doesn't need to be perfect |
| 5 | ICOM IC-705 | Receiver | FM on 437.550 MHz, recording the audio |
| 6 | Laptop running Hamlib | Rig control and the tracking script | `rigctld` for the radio, plus the Python tracking script |

These CCTV mounts usually ship with an RS-485 interface. I had the option to buy one with
a UART interface instead, so hooking the mount up to the ESP32 was straightforward: just
TX, RX and ground, at 3.3 V.


<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/block-system.png" width="90%" alt="System block diagram: laptop to ESP32 and mount over WiFi, laptop to IC-705 over USB" />
  <figcaption><em>The laptop does two jobs: it sends the ISS position over WiFi to the ESP32, which points the antenna, and it sends the Doppler-corrected frequency over USB to the IC-705.</em></figcaption>
</figure>

**The mount's reach.** The mount's tilt range is only about 50°, and out of the box most
of it points down, because it's built for a camera on a ceiling or a wall. I designed a
3D-printed bracket that tilts the antenna up, so the boom covers **15° to 65° of
elevation**. That means the antenna can't follow an overhead pass all the way through: it
stops at 65°. Pan is about 225°, so there is also a dead arc behind the
mount where it can't point.

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/mount-reach.png" width="100%" alt="The mount's tilt range before and after the bracket, and its pan range" />
  <figcaption><em>Left: the stock mount tilts from −32° to +18°, mostly below the horizon. Middle: the bracket raises the antenna by 46.5°, to 15°–65°. Right: pan covers 225°, leaving a 135° dead arc behind the mount.</em></figcaption>
</figure>

#### **Software**

- **Hamlib** standardises rig control (`rigctl`) and antenna rotator control (`rotctl`),
  but it doesn't support Pelco-D. **The ESP32 bridges the two over WiFi:** it runs a
  `rotctld`-compatible server, so any Hamlib client (`rotctl`, Gpredict and so on) can
  connect to it like a normal rotator.
- **The mount has no encoders and can't report its position.** The ESP32 works it out from how long it ran each motor and at what speed (dead reckoning), using speeds I measured on the bench.

- **The 9-DOF sensor** gives some feedback on the mount's movement and orientation. The accelerometer measures the antenna's elevation from gravity, which I use to calibrate the elevation.
  
- **Doppler.** For UHF satellites, tracking the position isn't enough. The ISS moves fast enough that the downlink shifts by about ±10 kHz at 437.550 MHz, so the receiver has to retune throughout the pass. Hamlib's `rigctld` sends each new frequency to the IC-705.
  
- **A Python script** (`sat-track`) picks an ISS pass, works out where the ISS is every
  second, and sends the commands: the position to the rotator over WiFi and the corrected
  frequency to the radio.

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/block-cycle.png" width="90%" alt="Flow of one tracking cycle: one prediction feeds both pointing and Doppler correction" />
  <figcaption><em>What happens every second of a pass: one prediction feeds both the antenna pointing and the Doppler correction.</em></figcaption>
</figure>

I developed the software with Claude Code.

# **The Satellite Pass**

The pass: **4 October 2026, 19:51 SGT**, rising in the north-west, peaking at **67°** at
19:56, and setting in the south-east at 20:02. It lasted about 11 minutes.

**Finding north:** The magnetometer readings were quite noisy, and the mount's steel and other nearby metal threw them off. So I laid my phone compass along the antenna boom to set the starting bearing, and let the mount work from there.

**Choosing the pass:** I set the mount up on my balcony, so I only picked passes that suited the location and the part of the sky I can see from there.

**Video:** The pass was also a visible one, so you could see the ISS move across the sky. Unfortunately the video doesn't capture it clearly, because the camera kept focusing on the antenna boom.

<iframe width="100%" height="315" src="https://www.youtube.com/embed/6MT-CS04LcY" title="ISS pass tracked with a CCTV pan-tilt mount" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# **Results**

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/iss-vs-boom.png" width="90%" alt="ISS azimuth and elevation against the antenna's pointing through the pass" />
  <figcaption><em>Where the ISS was (blue) and where the antenna was pointing (orange, the ESP32's own estimate), through the pass. The grey bands are where the mount can't reach. The grey column is a gap where the tracking script crashed on a WiFi stall and I restarted it.</em></figcaption>
</figure>

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/sky-reach.png" width="70%" alt="Sky plot of the pass with the mount's reachable area shaded" />
  <figcaption><em>The same pass on a sky plot (north up, the horizon at the edge, overhead in the centre). The shaded area is the part of the sky the mount can reach from where it stood.</em></figcaption>
</figure>

- **The tracking worked.** While the ISS was within reach, the antenna stayed within **1.7°** of it (median), and within 2.8° for 90 % of the time. With a beam about 60° wide, that's well under 1 dB of loss.
- **The Doppler tuning worked.** Whenever the script was running, the radio stayed within about 240 Hz of the right frequency. That's small compared to the FM signal's ±5 kHz deviation.
- **The mount could only reach the ISS for about 2.7 of the 11 minutes.** The ISS rose in the north-west, in the mount's dead arc, so the mount waited at its edge until the ISS came round. The balcony faces east, so the building blocked that part of the sky anyway. It went above 65° for about 20 s at the peak, and once it dropped below 15° the antenna just held at its lowest elevation.
- **A surprise: I still received SSTV with the ISS close to the horizon, well below 10° elevation.** At that point the antenna was held at its lowest elevation (about 16°), so it was pointing up to 13° above the ISS, and the wide beam did the work. The ISS SSTV downlink is quite strong: the UHF SSTV downlink typically runs at 10 W.

#### **Decoded SSTV images**

<figure>
  <img src="/assets/2026_10_11_iss_tracker_poc/sstv-1.jpg" width="100%" alt="Decoded SSTV image: Italian students spelling ARISS in a schoolyard" />
  <br><br>
  <img src="/assets/2026_10_11_iss_tracker_poc/sstv-2.jpg" width="100%" alt="Decoded SSTV image: ARISS-EA Virtual IORS" />
  <figcaption><em>Two slides from the ARISS "Student Education 2026" series (NA1SS / RS0ISS). Top: the first image received. Bottom: the second image is noisier, as the ISS was low on the horizon by then.</em></figcaption>
</figure>

# **Conclusion**

We have seen how a small CCTV pan-tilt mount can track the ISS and keep a Yagi pointed at it, while the radio follows the Doppler shift. A fuller write-up of the satellite tracker will follow as a series.


#### **Resources**


- [Hamlib](https://hamlib.github.io/)
- [ARISS](https://www.ariss.org/)
- [ARISS SSTV Series 33 announcement](https://issfanclub.eu/2026/09/24/ariss-sstv-event-starting-2-october-2026-900-utc-ending-6-october-2026-time-tbd/)
