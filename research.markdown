---
layout: page
title: Research
permalink: /research/
---

My doctoral research focused on developing novel sensing solutions for condition monitoring of large electrical machines, done at the [Rolls-Royce@NTU Corporate Lab](https://www.linkedin.com/company/rolls-royce-ntu-corporate-laboratory/about/){:target="_blank"}. The work centred on understanding the physics of failure, identifying suitable sensing modalities, and assessing both the feasibility of wireless sensor data access and the effectiveness of those signals in condition monitoring applications.

Conventional condition monitoring methods infer faults indirectly, from terminal signals like current signature analysis, vibration, and temperature, which only shift once a fault has grown large. My approach was to sense inside the machine instead, close to where faults originate, to catch the earliest signatures directly.

Two projects came out of this: a wireless system that made rotor-internal signals accessible for the first time, and stator end-winding sensor arrays that read inter-turn faults straight off the winding.


---

### Wireless Condition Monitoring of Brushless Synchronous Generators
Doctoral thesis
{: .proj-sub}

![Wi-CMS RF link inside the machine](/assets/research/proj1_wicms_link.png)
*Wireless Condition Monitoring System (Wi-CMS) Overview.*

![Rotor-mounted wireless sensor node](/assets/research/proj1_rts.png)
*Rotor Telemetry System mounted onto the BLSG machine.*
![Results comparison](/assets/research/proj1_results.png)
*Comparing fault detection performance between conventional and Wi-CMS.*
{: .proj-figs-right}

Brushless synchronous generators (BLSGs) are widely used for critical power generation, where an unexpected failure means expensive downtime and safety risk. Of the three most critical electrical faults (rotor field-winding, rotating-rectifier, and stator armature-winding faults), the first two originate on the *rotor*, a spinning body that conventional monitoring can only observe indirectly through stator-side signals. That indirection blurs the earliest fault signatures.

I proposed a Wireless Condition Monitoring System (Wi-CMS) that places wireless sensors directly on the rotor to access its electrical signals. Two challenges had to be addressed: establishing a reliable communication link, and demonstrating that the added complexity was justified by improved diagnostic performance. A feasibility study on IEEE 802.15.4 2.4 GHz radios showed that link quality inside the machine does not degrade, and in some conditions is measurably better, clearing the way to instrument a live rotor. I then developed a phase-domain fault model of a salient-pole BLSG, validated it against a 14 kVA test rig, and proposed new rotor-signal fault indicators that proved more sensitive than conventional terminal-based ones. Feeding these indicators into an SVM-based hybrid diagnostic scheme delivered a significant gain in multi-fault diagnostic accuracy, particularly at incipient fault stages.

---
{: .clear}

### Stator End-Winding Sensor Arrays
Infrared thermopile and Hall-effect arrays for inter-turn fault detection
{: .proj-sub}




![End-winding thermal and magnetic sensor array](/assets/research/proj2_endwinding_array.png)
*Circular sensor array facing the end-winding region and the spatial fault map it produces.*


![Spatial fault map from the array](/assets/research/proj2_fault_map_thermal.png)
*Thermal camera view vs end-winding temperature distribution during a stator inter-turn fault.*
![Spatial fault map from the array](/assets/research/proj2_fault_map_hall.png)
*Spatial magnetic flux deviation: healthy vs stator inter-turn fault.*
{: .proj-figs-left}

Stator inter-turn faults (a short between adjacent turns in a winding) start small but can cascade to a full winding failure. Conventional methods detect them through secondary effects on external signals like motor currents, vibration, and casing temperature, which can be both indirect and late. I wanted to measure the fault where it actually shows itself: at the end windings.

I proposed two circular sensing arrays mounted along the inner wall of the stator casing, facing the end-winding region: an infrared thermopile array (IRSA) and a Hall-effect array (HESA). Both measure non-contact, one mapping the temperature distribution and the other the magnetic flux distribution around the end winding. An inter-turn short breaks the thermal and magnetic symmetry of the winding, and the arrays read that deviation directly, giving an intuitive, spatial picture of the fault rather than an inferred one. Demonstrated on a 1.5 kW induction-motor test rig, the HESA proved the more versatile of the two, with better early-detection capability, so I built and validated an embedded online monitoring algorithm around it.



---
# Publications

### PhD Thesis

**[Condition Monitoring of Brushless Synchronous Generators using Wireless Sensors](https://dr.ntu.edu.sg/entities/publication/a76ea39f-fd2b-4f71-bfc0-4f25ff88d0ed){:target="_blank"}**
{: .pub-title}

Sampath Kumar Padmanabhan
{: .pub-authors}

Doctoral Thesis, Nanyang Technological University, Singapore, 2019
{: .pub-venue}

---

### Journal Articles

**[Stator end-winding thermal and magnetic sensor arrays for online stator inter-turn fault detection](https://drive.google.com/file/u/0/d/1Hm6ho72JOhaJciPWTFOIeM_cxMjhr0Qu/view){:target="_blank"}**
{: .pub-title}

**PS Kumar**, L Xie, MSM Halick, V Vaiyapuri
{: .pub-authors}

*IEEE Sensors Journal 21 (4), 5312–5321, 2021*
{: .pub-venue}

**[Winding fault diagnosis and failure prognosis technique for brushless synchronous generator](https://www.tandfonline.com/doi/full/10.1080/15325008.2020.1831652){:target="_blank"}**
{: .pub-title}

V Jaiswal, D Wang, **PS Kumar**
{: .pub-authors}

*Electric Power Components and Systems 48 (11), 1159–1170, 2020*
{: .pub-venue}

**[Feasibility for utilizing IEEE 802.15.4 compliant radios inside rotating electrical machines for wireless condition monitoring applications](https://drive.google.com/file/u/0/d/1zqZo8mktJlp0_EvW-kCVvgMvom3k73Zz/view){:target="_blank"}**
{: .pub-title}

**PS Kumar**, L Xie, BH Soong, MY Lee
{: .pub-authors}

*IEEE Sensors Journal 18 (10), 4293–4302, 2018*
{: .pub-venue}

---

### Conference Papers

**[Online stator end winding thermography using infrared sensor array](https://ieeexplore.ieee.org/document/8341361){:target="_blank"}**
{: .pub-title}

**PS Kumar**, L Xie, MSM Halick, V Vaiyapuri
{: .pub-authors}

*IEEE Applied Power Electronics Conference and Exposition (APEC), 2018, pp. 2454–2459*
{: .pub-venue}

**[Brushless synchronous generator turn-to-turn short circuit fault detection using multilayer neural network](https://dr.ntu.edu.sg/bitstreams/da078f19-cc4c-47d2-8eed-d030fa898130/download){:target="_blank"}**
{: .pub-title}

PP Tun, **PS Kumar**, RA Pratama, L Shuyong
{: .pub-authors}

*Asian Conference on Energy, Power and Transportation Electrification (ACEPT), 2018, pp. 1–8*
{: .pub-venue}

**[Online junction temperature for off-the-shelf power converters](https://ieeexplore.ieee.org/document/8341409){:target="_blank"}**
{: .pub-title}

MHM Sathik, S Prasanth, F Sasongko, **SK Padmanabhan**, J Pou, R Simanjorang
{: .pub-authors}

*IEEE Applied Power Electronics Conference and Exposition (APEC), 2018, pp. 2769–2774*
{: .pub-venue}

**[A dynamic thermal controller for power semiconductor devices](https://ieeexplore.ieee.org/document/8341413){:target="_blank"}**
{: .pub-title}

MHM Sathik, S Prasanth, F Sasongko, **SK Padmanabhan**, J Pou, R Simanjorang
{: .pub-authors}

*IEEE Applied Power Electronics Conference and Exposition (APEC), 2018, pp. 2792–2797*
{: .pub-venue}

**[Rotor mounted wireless sensors for condition monitoring of brushless synchronous generator](https://ieeexplore.ieee.org/document/8216544){:target="_blank"}**
{: .pub-title}

**PS Kumar**, L Xie, K Thiha, BH Soong, V Vaiyapuri, S Nadarajan
{: .pub-authors}

*43rd Annual Conference of the IEEE Industrial Electronics Society (IECON), 2017, pp. 3221–3226*
{: .pub-venue}

**[Feasibility of wireless RF communication inside rotating electrical machines for condition monitoring applications](https://www.researchgate.net/profile/Padmanabhan-Sampath-Kumar-2/publication/327510468_Feasibility_of_wireless_RF_communication_inside_rotating_electrical_machines_for_condition_monitoring_applications/links/5d497d0592851cd046a68329/Feasibility-of-wireless-RF-communication-inside-rotating-electrical-machines-for-condition-monitoring-applications.pdf){:target="_blank"}**
{: .pub-title}

**PS Kumar**, L Xie, BH Soong
{: .pub-authors}

*19th International Conference on Electrical Machines and Systems (ICEMS), 2016, pp. 1–6*
{: .pub-venue}

**[Modified winding function approach to stator fault modelling of synchronous generator](https://www.researchgate.net/profile/Padmanabhan-Sampath-Kumar-2/publication/305674745_Modified_winding_function_approach_to_stator_fault_modelling_of_synchronous_generator/links/5d497c7f4585153e59410bb0/Modified-winding-function-approach-to-stator-fault-modelling-of-synchronous-generator.pdf){:target="_blank"}**
{: .pub-title}

**PS Kumar**, Y Chen, MY Lee, S Nadarajan, L Xie
{: .pub-authors}

*12th IEEE International Conference on Control and Automation (ICCA), 2016, pp. 161–166*
{: .pub-venue}
