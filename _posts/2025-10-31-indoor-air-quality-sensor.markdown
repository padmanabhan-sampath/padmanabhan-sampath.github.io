---
layout: post
title: "Building a Custom Indoor Air Quality Sensor with ESP32, SEN66 and Home Assistant"
date: 2025-10-31
tags: [IoT, air-quality, ESP32, SEN66, home-automation, diy, sensors, home-assistant]
---


<p align="center">
   <img src="/assets/2025_10_31_iaq_sensor/iaq_sensor_setup.png" alt="ESP32-C6 IAQ Sensor with GC9A01A Display" width="400">
</p>

# **TL;DR**

>> - This post documents my journey building a custom IAQ sensor using **SEN66**, **ESP32-C6**, and **Home Assistant** integration.
>> - Real-time monitoring and tracking of PM1.0/2.5/4.0/10, VOC, NOx, CO₂, temperature, and humidity.
>> -  **Project Repository:** [GitHub - ESP32 SEN66 IAQ Sensor](https://github.com/padmanabhan-sampath/Indoor-Air-Quality-Sensor-ESP32-SEN66){:target="_blank"} 

# **Table of Contents**

1. [Introduction](#introduction)
2. [State of the Art: Why Build Your Own?](#state-of-the-art-why-build-your-own)
3. [Design Rationale](#design-rationale)
4. [How to Make It Work](#how-to-make-it-work)
5. [Results and Future Work](#results-and-future-work)
6. [Conclusion](#conclusion)

# **Introduction**

I wanted to build an air quality sensor to help track air quality when working beside a 3D printer, and to determine if indoor air quality is the reason for occasional sneeze fits and allergies. The key parameters I wanted to monitor are Volatile Organic Compounds (VOC) and Particulate Matter (PM). VOCs are sensitive to a wide array of chemical compounds, many common in households and completely benign. These are definitely expected to spike during 3D printing as it essentially involves melting plastics, creating the characteristic strong smells. PM is more related to dust and pollens.

**Why build my own sensor?**

I had explored commercial solutions and found that they are either too expensive or of questionable accuracy. I also wanted to add a level of customization, so an open-source solution would be more suitable. I wanted the sensor to integrate well with the locally run home automation suite I had.

There are plenty of open-source IAQ monitors that are commercially available as well. Most are still using discrete sensors, compared to the fully integrated offering from [Sensirion's SEN6x series](https://sensirion.com/products/catalog/SEN66/){:target="_blank"}. But I believe this will soon catch up as the SEN6x series offers the most compact form factor.


#### **SEN6x Series Brief**

The **SEN66** sensor provides comprehensive air quality monitoring in a single module:

| Sensor Type | Parameters | Range |
|-------------|------------|-------|
| **Particulate Matter** | PM1.0, PM2.5, PM4.0, PM10 | 0-1000 μg/m³ |
| **Gas Sensors** | VOC Index, NOx Index | 1-500 |
| **Environmental** | Temperature, Humidity | -40-85°C, 0-100% |
| **CO₂** | Concentration | 400-40,000 ppm  |


Here's a comparison of existing opensource IAQ sensors:

| Product | Cost | Size | Parameters | Open Source | Mobile App | Home Assistant | Notes |
|---------|------|------|------------|-------------|------------|----------------|-------|
| **[AirGradient](https://www.airgradient.com/){:target="_blank"}** | $230+ | Medium | PM2.5, CO₂, TVOC, NOx, Temp, Humidity | Yes | Yes | Yes | Popular, good community |
| **[AIR-1](https://apolloautomation.com/products/air-1){:target="_blank"}** | $130+ | Small | PM1/2.5/4/10, VOC, NOx, CO₂, Temp, Humidity | Yes | via HA | Yes | Compact |
| **[GAIA A08](https://aqicn.org/gaia/#A08){:target="_blank"}** | $100+ | Small | PM1.0/2.5/10, Temp, Humidity| Yes | Via Web | Yes | DIY, Arduino compatible |
| **[ESPHome IAQ](https://esphome.io/){:target="_blank"}** | DIY | - | Configurable (various sensors) | Yes | Via HA | Yes | Requires assembly |


# **Sensor Design**

I designed the sensor with the following requirements in mind:

1. **Periodically send IAQ data** for storage and analysis
2. **Display data locally on device**
3. **Wireless data transfer** via home WiFi network
4. **Local data hosting** with Home Assistant integration
5. **Multi-device support**: easy to add more devices for whole house coverage
6. **Experimental support**: easy to extend functionality with more sensors

#### **Bill of Materials**

| Component | Price (USD) | Notes |
|-----------|-------------|-------|
| ESP32-C6-DevKitC-1 | $7 | Low cost, readily available, WiFi 6 support, Thread/Matter compatible, sufficient GPIO |
| Sensirion SEN66 | $54 | All-in-one solution, proven Sensirion accuracy, I2C interface, compact 40x40x14mm |
| GC9A01A LCD (240x240) | $4 | Low cost, modern round form factor, good library support |
| General PCB/Jumpers | $5 | Prototyping connections |
| Enclosure | - | 3D printed|
| **Total** | **~$70** | Includes shipping |


# **Current Status**


<figure style="text-align: center;">
   <img src="/assets/2025_10_31_iaq_sensor/prototypes_evolution.png" width="100%" />
   <figcaption>Evolution of IAQ sensor prototypes during development</figcaption>
</figure>

As I want the design to serve as an experimental platform, all the components are simply connected via jumper wires. The 3D printed enclosure helps to neatly contain the setup. The left image shows proto-0, which was built to test the SEN66 and HA integration. It's still going strong after 1.5 months. The right side shows proto-1 (current version). The jumpers sticking out on top are connected to an ambient light sensor. The next iteration will include that sensor and a better 3D enclosure. I had intended to build 3 sensors, so the next one would be the final version. I will update this post with proto-2 iteration and future feature updates as well.


**What's Working:**
- Real-time sensor data collection (30-second intervals)
- Local LCD display  
- MQTT publishing to Home Assistant
- Multi-device support with unique identification
- Automatic WiFi reconnection and error handling


<figure style="text-align: center;">
   <img src="/assets/2025_10_31_iaq_sensor/ha_iaq_snippet.png" width="100%" />
   <figcaption>IAQ data display in Home Assistant homepage</figcaption>
</figure>

# **Next Steps**

1. **Add ambient light sensor** for ambient light levels tracking
2. **Improve enclosure design** with better ventilation and mounting options
3. **Implement sensor calibration routines** for improved accuracy
4. **Develop IAQ data analysis** to explore data patterns
   



# **Conclusion**

Building a custom IAQ sensor proved to be both educational and practical. The **SEN66 + ESP32-C6** combination provides a powerful, cost-effective platform for comprehensive air quality monitoring. 

The total project cost of ~$70 compares favorably to commercial alternatives while providing complete customization and open-source transparency. Looking forward to the next design iteration and analyzing the data patterns.

---

