---
layout: post
title:  "Receiving SSTV from the International Space Station with Yagi-Uda antenna"
excerpt: "A guide for receiving SSTV images from the International Space Station using a directional antenna, handheld VHF radio and an android phone"
date:   2024-05-26 19:30:55 +0800
categories: jekyll update
---

<head>
   <link rel="stylesheet" href="/css/main.css">
</head>


In the previous post, [Receiving SSTV from the International Space Station with minimal setup](https://padmanabhan-sampath.github.io/jekyll/update/2023/03/26/iss-sstv.html), we looked at receiving and decoding SSTV transmissions with just an android phone and an amateur radio handheld  transceiver (HT).  In this post, lets add in a directional (Yagi-Uda) antenna and wired audio interface circuit to improve the signal reception.

A directional antenna provides higher gain compared to an omnidirectional antenna, albeit in a specific direction. Higher gain helps to amplify and pull-out weaker signals. ISS-SSTV data is transmitted at whopping 25W, so even a simple rubber ducky antenna would suffice. The setup described here can serve as an entry to work other ISS radio modes and more importantly other AMSATs which typically have much lower transmit power (<1 W). A high gain antenna would be crucial for such AMSAT operations.

The audio interface circuit feeds the HT’s audio directly into the phone’s mic jack, thereby helping to eliminate interference from external ambient sounds.

> The audio interface circuit will need to be soldered together.


* TOC
{:toc}


# **Quick reference**

* Radio settings
  * VFO frequency: 145.800 MHz
  * Squelch: minimum level
  * Volume: 50% (make sure it does not saturate phone's audio input)
* Android apps
  * [Robot36 App][robot36-playstore]{:target="_blank"} 
    * Decoding mode: Auto or PD120
  * [Heavens-Above App][heavens-above-app]{:target="_blank"} 
    * Set location
    * Set ISS as radio satellite
* ISS status check
   1. [ARISS SSTV Blogspot][ariss-sstv-blogspot]{:target="_blank"} 
   2. [ARISS current status page][ariss-iss-current-status]{:target="_blank"} 
   3. [AMSAT Live status page][amsat-status]{:target="_blank"}: For SSTV transmissions look under ISS-DATV.


# **Hardware requirements**
1. VHF amateur handheld transceiver (HT)
2. Android phone with the following apps installed:
   - [Robot36][robot36-playstore]{:target="_blank"}: For SSTV audio decoding 
   -  [Heavens-Above][heavens-above-app]{:target="_blank"}: To predict the time and location of next ISS pass and for live-tracking
3. 3-element Yagi-Uda antenna
4. HT to phone audio interface circuit 


  <figure>
   <img src="/assets/2024_05_26_iss_sstv_yagi/overall-BD.png" width="80%" />
   <figcaption> Overall system block diagram.</figcaption>
</figure>

<figure>
  <img src="/assets/2024_05_26_iss_sstv_yagi/Full-setup.jpeg" width="100%" class="center" />
   <figcaption> Hardware needed: handeld radio, Yagi antenna, audio interface circuit, and an android phone</figcaption>
</figure>


# **Hardware setup**

**3-element Yagi-Uda antenna**<br>

   > The Yagi-Uda antenna was invented by Shintaro Uda of Tohoku Imperial University, Japan, in 1926, with a lesser role played by Hidetsugu Yagi. However, the name Yagi has become more familiar, while the name of Uda, who applied the idea in practice or established the conception through experiment, is often omitted. Source: [Wikipedia article](https://en.wikipedia.org/wiki/Yagi%E2%80%93Uda_antenna.)

   The Yagi-Uda antenna provides directivity and higher gain. This can be illustrated with the antenna radiation pattern charts shown below. Here we compare Yagi-Uda antenna with two omnidirectional antennas: monopole and dipole. We can observe that Yagi antenna provides increased gain, helping to amplify weaker signals in a specified direction, however it comes at the cost of decreased gain in other directions.


<figure>
  <img src="/assets/2024_05_26_iss_sstv_yagi/antenna_patterns.png" width="100%" class="center" />
   <figcaption> Antenna radiation patterns</figcaption>
</figure>

The 3-element Yagi-Uda antenna is the most widely used directional antenna for VHF Amateur radio Satellite operations. These antennas are commercially available from brands such as [Arrow Antennas]( https://www.arrowantennas.com/arrowii/146-3ii.html) [Elk Antennas]( https://elkantennas.com/). You can also easily build this antenna from common items available from local hardware stores for a fraction of the costs. One such ubiquitous home build is a ‘Tape measure Yagi antenna’. A Google search on this will return plenty of results with detailed build instructions. 

I built my own variant using telescopic radio antennas - my aim was to create an antenna that is light weight and foldable. More details can be found here: [Backpack-Yagi-Uda-Antenna](https://github.com/padmanabhan-sampath/Backpack-Yagi-Uda-Antenna).



**Audio interface**<br>

   <figure>
     <img src="/assets/2024_05_26_iss_sstv_yagi/audio-sch.png" width="60%" class="center" />
      <figcaption> Antenna radiation patterns</figcaption>
   </figure>

   The purpose of this circuit is to directly feed the HT audio into a phone’s audio jack. This avoids external ambient noise from interfering with SSTV decoding, which is critical for other ISS modes like the APRS. I repurposed the audio interface circuit designed for Baofeng UV5R radio [BaofengUV5R-TRRS](https://github.com/johnboiles/BaofengUV5R-TRRS)
   
   > You need to identify the MIC, speaker and GND outputs from the audio jack specific to your HT. Unfortunately, this is not standardized and differs among manufacturers.
   
 **Full signal chain test**<br>
   Once you have all the components in place, do a full signal check to see if the signal reception through the image decoding is being performed correctly. 

# **ISS SSTV reception**<br>

   Please refer to the previous post on selecting the right pass and app settings: [Receiving SSTV from the International Space Station with minimal setup](https://padmanabhan-sampath.github.io/jekyll/update/2023/03/26/iss-sstv.html)


   With the Yagi antenna, if you can mount your phone on the antenna (like in the picture below), satellite tracking just comes down to pointing the antenna such that the crosshair of the Heavens-above app stays close to the ISS icon.

   <figure>
     <img src="/assets/2024_05_26_iss_sstv_yagi/Full-setup-mobile.png" width="60%" class="center" />
      <figcaption> Yagi antenna with phone holder</figcaption>
   </figure>

# **Comparing two setups**

Here, I compare the choice of location on the signal reception. Ideally, we want to have a clear line of sight of the satellite, once it appears above the horizon. Even though the satellite symbol pops up in the Heavens-above’s live sky chart, the path should not be blocked by buildings and other terrain, as this leads to poor or no signal reception. Here I compare the signal reception with two similar setups with the key difference being the location. 

Thanks to Mr. Chew, call sign 91VYP, for giving me permission to use his setup and images.

| Call sign       | 9V1YP                                                | 9V1DT                            |
| --------------- | ---------------------------------------------------- | -------------------------------- |
| HT              | Kenwood TH-D74A                                      | Yaesu FT-60R                     |
| Antenna         | Arrow dual band                                      | Homebrew 3-el Yagi-Uda           |
| Decoding        | Decoded offline                                      | Live - Robot36 Android App       |
| Audio interface | HT built-in audio record                             | HT – Phone Mic in interface      |
| Location        | Roof top of multistorey car park – in city landscape | Hill top – clear view of horizon |


 <figure>
     <img src="/assets/2024_05_26_iss_sstv_yagi/sstv_yagi_equip_collage.png" width="80%" class="center" />
      <figcaption>Equipment and location comparison: Left column is from 9V1YP, right is from 9V1DT</figcaption>
</figure>

<figure>
   <img src="/assets/2024_05_26_iss_sstv_yagi/sstv_yagi_images_collage.png" width="100%" class="center" />
   <figcaption> SSTV received images comparison: Left column is from 9V1YP, right is from 9V1DT</figcaption>
</figure>


* Both Chew and I are within the same pass coverage, just about 20 kms apart in Singapore
* Chew's reception is from an urban location surrounded by buildings. In contrast, my location was on hill top with clear view of the horizon. This helps to maintain a longer unobstructed radio link to the satellite. This is clearly evident from the 8/12 image reception.
* Identifying a good QTH (location) increases the chances of a strong signal reception. 

> Please note that in thunderstorm prone areas, make sure you have a rain shelter nearby, and avoid operating you radio outdoor during a thunderstorm, as this poses lightening risks.

This concludes this post. We have seen how to perform SSTV reception with a Yagi antenna, handheld transceiver and an android phone. The next post will focus on using a similar setup for ISS-APRS operation.


[ariss-sstv-blogspot]: http://ariss-sstv.blogspot.com/ 
[sstv-gallery]: https://www.spaceflightsoftware.com/ARISS_SSTV/index.php
[amsat-status]: https://www.amsat.org/status/
[ariss-iss-current-status]: https://www.ariss.org/current-status-of-iss-stations.html
[ariss]: https://www.ariss.org/
[heavens-above-web]: https://www.heavens-above.com/
[heavens-above-app]: https://play.google.com/store/apps/details?id=com.heavens_above.viewer&hl=en&gl=US&pli=1
[robot36-playstore]: https://play.google.com/store/apps/details?id=xdsopl.robot36&hl=en&gl=US
[yt-pd120-audio-recordings]: https://www.youtube.com/watch?v=dQw4w9WgXcQ
[amsat-sstv-page]: https://amsat-uk.org/beginners/iss-sstv/