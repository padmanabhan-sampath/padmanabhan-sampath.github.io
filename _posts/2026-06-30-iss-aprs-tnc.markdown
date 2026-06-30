---
layout: post
title: "A Low-Cost Bluetooth APRS TNC for Working ISS APRS"
date: 2026-06-30
tags: [APRS, ISS, Amateur Radio, AMSAT, satellites]
---

<head>
  <link rel="stylesheet" href="/css/main.css">
</head>

# A Low-Cost Bluetooth APRS TNC for Working ISS APRS

## Why this post

I first drafted this in 2021 when I wanted a simple, portable APRS TNC for satellite operations. Since then, low-cost HTs with APRS built in and a growing ecosystem of custom firmware have changed the landscape, but the core problem hasn't gone away: APRS still feels overwhelming for newcomers, especially APRS via satellite. This post is partly a walkthrough of the hardware I built, and partly a write-up of what I learned working the ISS APRS digipeater.

If you've looked into APRS, you've probably noticed the cost of entry is steep. Handhelds with APRS baked in are typically **~400 USD**, and standalone commercial TNCs, while more versatile, tend to start at **200 USD** and can be bulky. Most tutorials assume one of three things: an APRS-capable HT, a PC running an APRS client, or a commercial TNC. I wanted something different: a **low-cost, portable, fully wireless** setup I could carry in one hand and use to work APRS satellites.

What I ended up with is a **sub-10 USD Bluetooth APRS KISS TNC** built around an Arduino Nano clone.

---

## A quick APRS primer

APRS (Automatic Packet Reporting System) is an audio-modulated protocol for transmitting **location information and short messages** over radio. Because the data rides as audio tones, anything that can key a radio and play/decode audio can be used for APRS.

With free apps like [**APRSdroid**](https://aprsdroid.org), almost any Android phone can be used to generate APRS packets and push them out through a radio. If you have two HTs, you can start experimenting immediately.

APRS is used for:

1. Location tracking
2. **Satellite work** (the focus here)
3. Emergency and event communications

Behind it sits an infrastructure comprising:

1. **Digipeaters**: repeat packets over the air
2. **iGates / gateways**: feed packets to the internet, e.g. [aprs.fi](https://aprs.fi)

For a deeper APRS introduction, these two videos are excellent:

1. [APRS overview (YouTube)](https://www.youtube.com/watch?v=ULdCMPuQ8oY)
2. [APRS deep dive (YouTube)](https://www.youtube.com/watch?v=9CVKedg9Fxg)

The ISS carries an [APRS digipeater](https://www.ariss.org/current-status-of-iss-stations.html). A packet you transmit up to the ISS gets **repeated and re-transmitted** back down. Because the station has a huge radio footprint, lots of stations can hear that repeated packet, which means you can make **QSL contacts via APRS** through the satellite.

**What you need:**

- A 2 m (144 MHz) HT
- A directional antenna (a small Yagi makes pointing at the pass much easier)
- An APRS TNC (or, for the minimal setup, just an audio cable)

**The signal path looks like this:**

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/signal_path.png" width="90%" alt="Signal path: HT to ISS digipeater to ground iGate to aprs.fi" />
  <figcaption><em>Signal path: HT → ISS digipeater → ground iGate → aprs.fi.</em></figcaption>
</figure>

---

## Two setups: from minimal to TNC

I worked the ISS with two sets of gear. Here is a brief contrast:

| | **Minimal** | **Arduino TNC** |
|---|---|---|
| Signal chain | Phone → audio cable → HT → dipole | Phone →(Bluetooth)→ BT-TNC → HT → Yagi |
| PTT | Manual (push the button) | Automatic (TNC-controlled) |
| Receive | Effectively none | Possible, but poor on weak signals |
| Goal | Just hit the ISS | Two-way: transmit **and** decode |

In **both** cases I was able to hit the ISS. The difference was on receive: the minimal setup gave essentially **no** packet reception, while the Arduino TNC could receive, though **poorly when signals were weak**. Worth noting: the TNC decodes **strong** packets reliably; it only struggles as the signal drops off.

---

## First attempt (minimal setup): phone, audio cable, manual PTT

My first attempt was deliberately bare-bones:

- **Phone → audio cable → HT → dipole antenna**
- **Manual PTT**: I keyed the radio by hand around the ISS pass
- **Receive ignored**: the sole objective was to get a packet *up* to the ISS

This works for proving you can reach the satellite. You transmit during the pass, then check the internet logs afterward to confirm your packet was digipeated. It's a great, cheap way to get your first taste, but it's clumsy and tedious for repeated attempts.

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/minimal_setup_photo.jpg" width="50%" alt="Minimal setup: phone, audio cable, HT, dipole" />
  <figcaption><em>Minimal setup: phone, audio cable, HT, dipole.</em></figcaption>
</figure>

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/minimal_setup_aprs_map.jpg" width="100%" alt="aprs.fi map showing the digipeated packet from the minimal setup" />
  <figcaption><em>aprs.fi confirming the minimal-setup packet was digipeated by the ISS. Look for 9V1DT-7 in the image.</em></figcaption>
</figure>

---

## Building the Bluetooth APRS KISS TNC

This is the heart of the project. The TNC is a low-cost (**< 10 USD**) Bluetooth APRS KISS TNC built from:

1. Arduino Nano clone
2. HC-05 / HC-06 Bluetooth module
3. A handful of passive components

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/arduino_tnc_board.png" width="80%" alt="Assembled Bluetooth APRS KISS TNC built around an Arduino Nano clone" />
  <figcaption><em>The assembled Bluetooth APRS KISS TNC: Arduino Nano clone with HC-05 Bluetooth module and a small handful of passives.</em></figcaption>
</figure>

This build stands on the shoulders of two earlier projects:

- **MicroAPRS** ([unsigned.io/projects/microaprs](https://unsigned.io/projects/microaprs/)): an ATmega328-based KISS TNC over a serial interface.
- **VK3DAN** ([vk3dan.ninja](https://www.vk3dan.ninja/2017/08/03/homebrew-aprs-arduino-uno-kiss-tnc/)): added Bluetooth support to the TNC using an Arduino UNO.

My version (**9V1DT**) takes VK3DAN's work and adapts it to a more compact, field-friendly form factor.

### What I changed

- **Arduino Nano** instead of the UNO, for a much smaller footprint
- A **modified PTT circuit** for multiplexed HT operation

---

## Testing the TNC

I brought the TNC up in stages rather than all at once. It makes faults much easier to isolate:

1. **Serial interface test**: talk to the TNC over the serial port.
3. **Add the Bluetooth interface**: bring the HC-05/06 online and confirm wireless KISS comms.
4. **PTT test**: verify the TNC keys the radio correctly.
5. **Radio test**: if you have a spare radio, transmit and decode locally before going after the satellite.

---

## Second attempt (Gen 2): putting the TNC on the air

For my second attempt I used the Arduino TNC to handle the APRS packets, no more manual PTT. The TNC keys the radio automatically and handles both transmit and receive.

### The setup

- **3-element Yagi antenna**
- **HT radio**
- **Arduino Bluetooth TNC**
- **Android phone running APRSdroid**

The Bluetooth module is what makes this pleasant in the field. APRSdroid talks to the TNC directly over **Bluetooth using the KISS protocol**, so the link is fully wireless: you only need to hold the antenna and the phone, while the radio and TNC tuck neatly into a carry bag.

On the antenna: a directional Yagi makes it far easier to track and point at the passing ISS.

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/arduino_tnc_full_setup.png" width="60%" alt="Gen 2 field setup: Yagi, HT, Arduino Bluetooth TNC, phone" />
  <figcaption><em>Gen 2 field setup: Yagi, HT, Arduino Bluetooth TNC, Android phone.</em></figcaption>
</figure>

### APRSdroid configuration

In APRSdroid, the connection protocol is set to **TNC (KISS)** with the HC-05 module selected as the Bluetooth device. Callsign / SSID and the ISS-friendly digipath (`ARISS` / `WIDE1-1, WIDE2-2`) are configured in the connection preferences.

<figure>
  <img src="/assets/2026_06_30_iss_aprs_tnc/APRSdroid_settings.jpg" width="40%" alt="APRSdroid KISS-over-Bluetooth connection settings" />
  <figcaption><em>APRSdroid KISS-over-Bluetooth connection settings.</em></figcaption>
</figure>

### Radio settings

The HT is parked on the ISS APRS simplex frequency **145.825 MHz**, squelch open enough that the TNC sees the audio, and volume tuned by ear so the demodulated tones land cleanly into the Nano's ADC.

---

## Results from the field

On **2 May 2026**, fellow Singapore ham Chew (**9V1YP**) and I worked the same ISS pass from two different locations in Singapore. The aprs.fi logs below show our packets being received by the ISS digipeater (`NA1SS`) and relayed to ground iGates in Indonesia (`YC1SCC-10`, `YF1ZQA-6`, `YB1TJ-2`). One of them, `YD0NXX` in Jakarta, even messaged Chew back to confirm **"59 in Jakarta"**.

**Curated packet log, ISS digipeated traffic:**

| Time (UTC) | From | Via | Notes |
|---|---|---|---|
| 04:19:46 | 9V1DT-7 (me) | NA1SS → YC1SCC-10 (Jakarta iGate) | Position beacon digipeated by the ISS |
| 04:20:45 | 9V1DT-7 (me) | NA1SS → YC1SCC-10 | Position + comment "9V1DT Singapore" |
| 04:22:07 | 9V1DT-7 (me) | NA1SS → YF1ZQA-6 | Position beacon via a different iGate |
| 04:23:08 | 9V1DT-7 (me) | NA1SS → YF1ZQA-6 | Marked **"via ISS"** by aprs.fi |
| 04:20–04:23 | 9V1YP-7 (Chew) | NA1SS → YC1SCC-10 / YF1ZQA-6 | Multiple position + status packets digipeated |

**Curated message log, direct messages through the ISS:**

| Time (UTC) | From → To | Message |
|---|---|---|
| 04:21:01 | 9V1YP-7 → YD0NXX | "Have a good weekend OM" |
| 04:21:20 | 9V1YP-7 → YD0NXX | "Good day OM" |
| 04:21:41 | **YD0NXX → 9V1YP-7** | **"Hi ur 59 in Jakarta"** |
| 04:22:21 | **YD0NXX → 9V1YP-7** | **"Hi Chew ur 59 in Jakarta"** |
| 04:23:50 | 9V1YP-7 → YF1ZQA | "Have a good weekend OM" |
| 04:23:52 | 9V1YP-7 → YD0NXX | "73s" |

That YD0NXX → 9V1YP-7 reply is the moment that matters: a **real, two-way QSO via the ISS APRS digipeater**, made on a sub-10 USD homebrew TNC and a 3-element Yagi.

Across the whole interaction, though, I received essentially **no** packets directly from the ISS on my own setup. I had to check aprs.fi after the pass to know my packets had made it up. Bench checks with a second HT confirmed the Arduino TNC *does* decode correctly, but as the received signal gets weaker, decoding falls apart. My suspicion is the **low-resolution 8-bit ADC** on the Arduino.

A quick comparison against a **USB sound card running Direwolf** on the same pass showed the Arduino TNC's **receive performance was clearly poorer** than the USB modem.

---

## Lessons learned and next iteration

- **Transmit is the easy part.** Both setups reliably hit the ISS. Getting a packet *up* is forgiving.
- **Receive is where the hardware matters.** Weak-signal decoding, exactly the regime you're in with a satellite, is limited by the analog front end and the Arduino's 8-bit ADC.
- **Next iteration:** a TNC with **better analog stages** and higher-resolution sampling should meaningfully improve weak-signal reception. New projects using the **ESP32** look very promising: lower BoM, better ADC/DAC, more processing headroom, and Wi-Fi/Bluetooth built in.

---

## Resources

- MicroAPRS: [unsigned.io/projects/microaprs](https://unsigned.io/projects/microaprs/)
- VK3DAN homebrew APRS KISS TNC: [vk3dan.ninja](https://www.vk3dan.ninja/2017/08/03/homebrew-aprs-arduino-uno-kiss-tnc/)
- APRSdroid: [aprsdroid.org](https://aprsdroid.org)
- ARISS current status: [ariss.org](https://www.ariss.org/current-status-of-iss-stations.html)
- aprs.fi: [aprs.fi](https://aprs.fi)
