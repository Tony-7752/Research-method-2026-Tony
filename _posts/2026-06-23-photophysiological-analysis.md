---
layout: post
title: Photophysiological Analysis using R
date: '2026-06-23'
categories: Protocols
tags: Photosynthesis R 
---
### 23/06/2026
# Research methods course assignment
### *Analysis of algae samples PAM (Pulse-Amplitude Modulation) fluorometry data*

For our research methods course, we had a two part assignment:

* Field sampling - We went to Sdot Yam beach, and gathered algae samples for PAM analysis. Transects were used to estimate algae coverage.
* Data analysis - After we  recieved raw data, we had a workshop where we analysed the results in R to get meaningful insights.

#### Our goals in this study were to asses algae coverage at Sdot Yam beach and compare algae from different niches using their photosyntetic efficiency ratings.

This post will elaborate on the methods and analysis used in the assignment, interpret the results and suggest improvements for next year's assignment.

## 1. <u>Introduction</u>

During the field sampling, the area of interest was divided into transects where students measured coverage of different species of algae on the rocky shore. From these transects, algae samples were taken from observable high or low light conditions (*high light* - algae found at the surface of the rocky shore, *low light* - algae found deeper inside small pools on the shore). The samples were taken shortly after collection to the lab, and into the PAM device for analysis.

Pulse-Amplitude Modulation (PAM) fluorometry is a non-invasive technique used to measure chlorophyll fluorescence and assess plant or algae photosynthetic performance. It is a widely used method to determine photosynthetic efficiency, and in this experiment we have used it to analyze different algae species we have sampled on our course assignment.

## 2. <u>Materials & Methods</u>

### 2.1 Field survey and sample collection

Our area of interest was Sdot Yam beach, and particularly the rocky shore formations teeming with many species of algae. We diveded into several sub-groups of students, all tasked with a different direction and location of transect deployment for the algae coverage analysis:

1. North-to-South (N-S) 
2. South-to-North (S-N) 
3. East-to-West (E-W) 
4. West-to-East (W-E) 

Transect lines were deployed, and student groups were equipped with standard `1m × 1m` quadrats containing 25 sub-quadrats. Every group set out in a different direction and coverage of algae species was measured.

During this survey, samples of algae were also taken from several locations with alternating light conditions: high or low. Algae species were identified with the help of an algae identification card and course staff.

### 2.2 PAM fluorometry measurements

After sample acquisition, we headed out to the lab to test the samples in our PAM fluorometer. Algae samples where placed inside a petri dish and run in two cycles: 
* Algae gathered from high light areas  
* Algae from low light areas

![PAM.png](https://tony-7752.github.io/Research-method-2026-Tony/images/PAM.png)

PAM-2500, by WALZ

This device measures fluorescence emitted by the sample after it uses light pulses to excite it. To overcome the ambient light and have a true measurement of the algae/plant fluorescence, the PAM fires short pulses of light at specific wavelengths, and measures the returning fluorescence pulses through a long pass filter, thus overcoming the constant ambient fluorescence.

Data acquired from PAM measurments is used in our analysis with 4 main parameters:

1. $Am$ (Maximum Photosynthesis / $P_{max}$) - Max photosynthesis capacity, full receptor saturation.
2. $AQY$ (Photosynthetic Efficiency / $\alpha$) - The slope of the curve, indicates how fast and efficiently can the algae use available light.
3. $Ik$ (Minimum Saturating Irradiance) - The exact tipping point where the algae transitions from being "light-starved" to "light-saturated." calculated by $Am / AQY$, the optimal light intensity the algae is adapted to.
4. $Rd$ (Dark Respiration) - The y-axis intercept. Indicates the algae respiration in zero light conditions (negative photosynthesis).

![PI-graph.png](https://tony-7752.github.io/Research-method-2026-Tony/images/PI-graph.png)

Figure 1: P-I (photosyntesis-light intensity) plot

Another important measurment of photosynthetic activity is ETR (electron transport rate). It is calculated by:

$$ETR = PAR \times Y(II) \times 0.5 \times AF$$

1. $PAR$ - Photosynthetically Active Radiation. The amount of light hitting the algae.
2. $Y(II)$ or $\Phi_{PSII}$ - The effective quantum yield. The fraction of light that was used for photosynthesis, and not lost to heat disipation or fluorescence.
3. $AF$ (Absorption Factor): The fraction of light the algae physically absorbs instead of reflecting or letting pass straight through.

### 2.3 Data analysis

The output from the PAM device are two csv tables, light and dark.

