---
title: "Teardown: Meisei iMS-100 eco GPS radiosonde"
date: 2026-09-20
device: "Meisei iMS-100 eco"
summary: >-
  A recovered Singapore weather-balloon radiosonde, traced part by part to work out what each block actually does.
thumbnail: /assets/teardowns/radiosonde/unit.jpg
tags: [radiosonde, amateur-radio, sensors, RF, teardown]
---

I was lucky to get a radiosonde from 9V1CV, a fellow amateur in Singapore who is an expert at chasing these down and recovering them.

A radiosonde is a disposable weather station carried up by a high-altitude balloon. It reports temperature, humidity and GPS position once per second as it climbs to about 35 km, where the balloon bursts. Wind comes from the GPS track, and pressure is calculated from GPS altitude rather than measured. These measurements feed the weather forecast models.

In Singapore, launches are from the Upper Air Observatory at Upper Paya Lebar. Most sondes are never recovered.


<figure>
  <img src="/assets/teardowns/radiosonde/unit.jpg" width="85%" alt="Meisei iMS-100 eco radiosonde, side view with sensor boom and manufacturer label" />
  <figcaption><em>Meisei iMS-100 eco radiosonde, side view with sensor boom and manufacturer label.</em></figcaption>
</figure>

| At a glance | |
|---|---|
| Model | Meisei iMS-100 (eco), manufactured 2024 |
| Flown by | Meteorological Service Singapore |
| Sensors | Thermistor, capacitive humidity; pressure derived from GPS |
| Radio | 400–406 MHz, 1200 bps, ≤100 mW |
| Power | One CR123A lithium cell, about 4 hours |
| Mass | Under 50 g including battery |


## Meisei iMS-100 (eco) Radiosonde

The foam casing holds one PCB, a CR-2/3A (CR123A-size) lithium cell and a sensor boom that plugs into the board.

<figure>
  <img src="/assets/teardowns/radiosonde/battery.jpg" width="80%" alt="Side and top views of the board with the lithium cell mounted" />
  <figcaption><em>Side and top views of the assembly: the CR-2/3A cell held against the board by moulded foam, its two-pin lead into CN2, and the sensor boom arcing away from the tip.</em></figcaption>
</figure>

## Meisei datasheet

| Parameter | Datasheet value |
|---|---|
| Temperature | −95 to +60 °C, response < 0.4 s, uncertainty 0.4–0.8 °C (k=2) |
| Humidity | 0–100 %RH, uncertainty 5 %RH in the troposphere |
| Pressure | 1050–3 hPa, derived from GPS |
| Wind | 0.15 m/s from GPS velocity |
| GPS | 66 channels, SBAS |
| Transmitter | 404.5 MHz centre, 1200 bps digital PCM, ≤100 mW, > 250 km with Yagi |

## Teardown

<figure>
  <img src="/assets/teardowns/radiosonde/pcb-annotated.jpg" width="70%" alt="iMS-100 PCB with functional zones A to F and numbered components" />
  <figcaption><em>iMS-100 PCB with functional zones A–F and numbered components.</em></figcaption>
</figure>

| # | Part | Function | Confidence |
|---|---|---|---|
| 1 | Inpaq chip antenna | GPS L1 antenna | Marking |
| 2 | Position Co. GSU-141-2 | Multi-GNSS module with LNA and SAW filter | Marking |
| 3 | CN1 | Sensor boom connector | Silkscreen |
| 4 | Toshiba 74HCU04 | Unbuffered inverter, CR oscillator | Part from marking; role inferred |
| 5 | Toshiba 74HC4052 | Dual 4:1 analog mux | Part from marking; role inferred |
| 6 | Renesas R5F100GFD | RL78/G13 MCU, 96 KB flash | Likely (abbreviated marking) |
| 7 | IC4 | IrDA transceiver for pre-launch setup | Inferred (two optical windows) |
| 8 | Xilinx XC2C32A | CoolRunner-II CPLD, function unknown | Part from marking |
| 9 | CN2 | Battery connector | Silkscreen |
| 10 | 2× LTC3526LB | Fixed-frequency boost converters | Marking code "LCST" |
| 11 | S1 | Push switch | Visual |
| 12 | J5 | Unpopulated auxiliary sensor port | Inferred from datasheet |
| 13 | Silicon Labs Si4063 | 400 MHz transmitter, +20 dBm | Marking "40632A" |
| 14 | Y2 | Transmitter reference oscillator | Likely |
| 15 | Antenna feed | Wire monopole | Visual |

Still unidentified: IC13 and IC14 (SOT-23-5, marked QLDR and QLPR, probably regulators) and two programming headers, J6 and J7.

<figure>
  <img src="/assets/teardowns/radiosonde/chip-markings.jpg" width="90%" alt="Close-ups of the main IC markings" />
  <figcaption><em>Close-ups of the main IC markings.</em></figcaption>
</figure>

## System block diagram

<figure>
  <img src="/assets/teardowns/radiosonde/block-diagram.png" width="75%" alt="Block diagram populated with the identified parts" />
  <figcaption><em>Block diagram populated with the identified parts.</em></figcaption>
</figure>

## Sensor boom

<figure>
  <img src="/assets/teardowns/radiosonde/sensor-boom.jpg" width="75%" alt="Sensor boom and humidity sensor under the aluminised cap" />
  <figcaption><em>Sensor boom and humidity sensor under the aluminised cap.</em></figcaption>
</figure>

(a) Thermistor bead, about 0.43 mm and coated in aluminium and silica (per GRUAN TD-5), held on fine leads away from the boom. (b) Aluminised cap over the humidity sensor, to limit icing and solar heating. (c) Flexible boom with white solder resist, also to limit solar heating. (d) Contacts into CN1. (e) Gold two-lead header carrying the humidity sensor. (f) Thin-film capacitive element with its bond wire.


<figure>
  <img src="/assets/teardowns/radiosonde/sensor-views.jpg" width="70%" alt="Front, top and side views of the sensor boom" />
  <figcaption><em>Front, top and side views of the boom: gold contacts at one end, the aluminised cap over the humidity sensor, and the forked tip carrying the thermistor on fine leads.</em></figcaption>
</figure>

## Design observations

- **No ADC in the sensor path.** A mux and a CR oscillator turn resistance and capacitance into frequency, which GRUAN TD-5 confirms. It is cheap and tolerant of noise, and reference components can be switched through the same path.
- **A CPLD function is unkown.** Its role is not documented and I could not trace it from photos, so it stays unassigned in the diagram.
- **No external RF power amplifier.** The Si4063 tops out at +20 dBm, exactly the 100 mW spec. The 1200 bps rate trades throughput for range.
- **Fixed-frequency boost converters.** A 3 V lithium cell sags in the cold, so boosting keeps the rails stable. Fixed-frequency switching keeps its spurs predictable next to the GPS and RF sections.


## References

- [Meisei iMS-100 eco product page](https://www.meisei.co.jp/english/products/meteorology/upper-air/p3193){:target="_blank"} and [datasheet](https://www.meisei.co.jp/english/wp-content/uploads/2025/02/%E3%80%88MSPA4-108_M2501%E3%80%89GPS-Radiosonde-iMS-100eco.pdf){:target="_blank"}
- [GRUAN TD-5: Meisei radiosondes (Kizu et al., 2018)](https://www.gruan.org/documentation/gruan/td/gruan-td-5){:target="_blank"}
- [Meteorological Service Singapore: observing the weather](https://www.weather.gov.sg/learn_observations/){:target="_blank"}
- [Position Co. GSU-141](https://www.posit.co.jp/product/gsu-141.html){:target="_blank"}, [Silicon Labs Si4063](https://www.silabs.com/documents/public/data-sheets/Si4063-60-C.pdf){:target="_blank"}, [ADI LTC3526L/LB](https://www.analog.com/media/en/technical-documentation/data-sheets/3526lfc.pdf){:target="_blank"}, [Renesas RL78/G13](https://www.renesas.com/en/doc/products/mpumcu/doc/rl78/r01ds0131ej0340-rl78g13.pdf){:target="_blank"}
