---
layout: post
title: Photophysiological Analysis using R
date: '2026-06-23'
categories: Protocols
tags: Photosynthesis R 
---
<script>
  MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$', '$$'], ['\\[', '\\]']]
    }
  };
</script>
<script id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>

### 23/06/2026
# Research methods course assignment
### *Analysis of algae samples PAM (Pulse-Amplitude Modulation) fluorometry data*

For our research methods course, we had a two part assignment:

* Field sampling - We went to Sdot Yam beach, and gathered algae samples for PAM analysis. Transects were used to estimate algae coverage.
* Data analysis - After we  recieved raw data, we had a workshop where we analysed the results in R to get meaningful insights.

#### Our goals in this study were to assess algae coverage at Sdot Yam beach and compare algae from different niches using their photosynthetic efficiency ratings.

This post will elaborate on the methods and analysis used in the assignment, interpret the results and suggest improvements for next year's assignment.

## 1. <u>Introduction</u>

During the field sampling, the area of interest was divided into transects where students measured coverage of different species of algae on the rocky shore. From these transects, algae samples were taken from observable high or low light conditions (*high light* - algae found at the surface of the rocky shore, *low light* - algae found deeper inside small pools on the shore). The samples were taken shortly after collection to the lab, and into the PAM device for analysis.

Pulse-Amplitude Modulation (PAM) fluorometry is a non-invasive technique used to measure chlorophyll fluorescence and assess plant or algae photosynthetic performance. It is a widely used method to determine photosynthetic efficiency (Ralph & Gademann, 2005), and in this experiment we have used it to analyze different algae species we have sampled on our course assignment.


## 2. <u>Materials & Methods</u>

### 2.1 Field survey and sample collection

Our area of interest was Sdot Yam beach, and particularly the rocky shore formations teeming with many species of algae. We divided into several sub-groups of students, all tasked with a different direction and location of transect deployment for the algae coverage analysis:

1. North-to-South (N-S) 
2. South-to-North (S-N) 
3. East-to-West (E-W) 
4. West-to-East (W-E) 

Transect lines were deployed, and student groups were equipped with standard `1m × 1m` quadrats containing 25 sub-quadrats. Every group set out in a different direction and coverage of algae species was measured.

During this survey, samples of algae were also taken from several locations with alternating light conditions: high or low. Algae species were identified with the help of an algae identification card and course staff.

### 2.2 PAM fluorometry measurements

After sample acquisition, we headed out to the lab to test the samples in our PAM fluorometer. Algae samples were placed inside a petri dish and run in two cycles: 
* Algae gathered from high light areas  
* Algae from low light areas

![PAM.png](https://tony-7752.github.io/Research-method-2026-Tony/images/PAM.png)

PAM-2500, by WALZ

This device measures fluorescence emitted by the sample after it uses light pulses to excite it. To overcome the ambient light and have a true measurement of the algae/plant fluorescence, the PAM fires short pulses of light at specific wavelengths, and measures the returning fluorescence pulses through a long pass filter, thus overcoming the constant ambient fluorescence.

Data acquired from PAM measurements is used in our analysis with 4 main parameters (Falkowski & Raven, 2013):

1. $Am$ (Maximum Photosynthesis / $P_{max}$) - Max photosynthesis capacity, full receptor saturation.
2. $AQY$ (Photosynthetic Efficiency / $\alpha$) - The slope of the curve, indicates how fast and efficiently can the algae use available light.
3. $Ik$ (Minimum Saturating Irradiance) - The exact tipping point where the algae transitions from being "light-starved" to "light-saturated." calculated by $Am / AQY$, the optimal light intensity the algae is adapted to.
4. $Rd$ (Dark Respiration) - The y-axis intercept. Indicates the algae respiration in zero light conditions (negative photosynthesis).

![PI-graph.png](https://tony-7752.github.io/Research-method-2026-Tony/images/PI-graph.png)

Figure 1: P-I (photosyntesis-light intensity) plot

Another important measurment of photosynthetic activity is ETR (electron transport rate). It is calculated by:

$$ETR = PAR \times Y(II) \times 0.5 \times AF$$

1. $PAR$ - Photosynthetically Active Radiation. The amount of light hitting the algae.
2. $Y(II)$ or $\Phi_{PSII}$ - The effective quantum yield. The fraction of light that was used for photosynthesis, and not lost to heat dissipation or fluorescence.
3. $AF$ (Absorption Factor): The fraction of light the algae physically absorbs instead of reflecting or letting pass straight through.

### 2.3 Data analysis

Raw PAM output files (.csv) and metadata were imported into R (v4.6.0). To prepare the data for modeling, the dataset was filtered to remove instrumental errors (measurements where ETR fell to zero at PAR > 0) and transformed into a long format. Photosynthesis-Irradiance (P-I) curves were fitted to the ETR data using non-linear least squares regression to extract the parameters $Am$, $AQY$, $Rd$, and $Ik$ for each individual sample. Summary statistics (mean, median, standard deviation) were calculated for both the high-light and low-light adapted groups. Finally, to determine if the observed differences between the environmental niches were statistically significant, paired Wilcoxon tests were conducted on the extracted parameters, with p-values adjusted for multiple comparisons.

## 3. <u>Results</u>

### 3.1 P-I curves

After cleaning the data, we created P-I curves for dark and light environment algae.

![light-p-i.png](https://tony-7752.github.io/Research-method-2026-Tony/images/light-p-i.png)

Figure 2: Fitted P-I curves for algae taxa in high-light conditions. The algae species *Namaliun* was removed from the dataset due to its measurements being corrupted and remained at 0 for most of the experiment.

![dark-p-i.png](https://tony-7752.github.io/Research-method-2026-Tony/images/dark-p-i.png)

Figure 3: Fitted P-I curves for algae taxa in low-light conditions. 

Evidently, *Padina* had the highest maximum photosynthesis, and we can see most of the curves have reached saturation.

### 3.2 Comparison of High-Light vs. Low-Light Niches

To evaluate and compare the two niches, box-plots were created of the relevant parameters.

![boxplot.png](https://tony-7752.github.io/Research-method-2026-Tony/images/boxplot.png)

Figure 4: Distribution of the four extracted photophysiological parameters across the two environmental groups.

Visual inspection of the boxplots reveals several potential trends. $Am$ and $Ik$ both appear to be higher in high-light niches, which would correlate to what is known about high-light species (Johansson & Snoeijs, 2002). $AQY$ and $Rd$ values are both higher in low-light algae. While low-light algae are known to have higher $AQY$ to maximize scarce photon capture, $Rd$ is traditionally known to be higher in high-light algae due to the metabolic costs of maintaining a larger photosynthetic capacity (Boardman, 1977).

Statistical analysis and tests (Wilcoxon signed-rank test), however, indicate a non-statistically significant result.


| Parameter | Raw P-value (P) | Adjusted P-value (Padj) |
| --- | --- | --- |
| AQY | 	0.21875 | 0.2187500 |
| Am | 0.15625 | 0.2083333 |
| Ik | 0.09375 | 0.2083333 |
| Rd | 0.15625 | 0.2083333 |

Table 1: Wilcoxon signed-rank test results.

## 4. Discussion

### 4.1 Insignificant results

While the boxplots seem to indicate the results we expected, the statistical analysis results in us *not* being able to reject the null hypothesis (There is *no* difference of the tested parameters between low and high light environments).

### 4.2 Possible causes of statistical insignificance

The statistical insignificance can be explained by several factors that affect the experimental setup. Sample collection was done by several groups of students working separately. No coordination was done between student groups on what algae to gather or from where. Thus, we ended up with algae sample from what each group deemed high or low light independently, which could be different between the groups. An indication of that is the appearance of *Padina* in both groups. Correct identification of algae could also potentially be an issue, and it must be verified before running the experiment.

Another aspect of lack of coordination was apparent in the algae coverage using the quadrats, where each group gathered data independently and used different units. This caused the coverage data to be unusable and no analysis could be done with it.

### 4.3 Recommendations for next year's assignment

To improve the course and this assignment, it is advised to create a short protocol to be distributed to students before the field survey containing:

* General information about Sdot Yam where they will be going
* Present the experiment and survey they are about to do - gather algae species and measure algae coverage
* Highlight the importance of consistent data gathering - either give the students the units in which to measure coverage, or ask them all to agree on one metric
* Highlight the importance of sample collection consistency - instead of each group gathering what they want, designate one group with clearer instructions on sample gathering (clear distinction between light and dark niches, no species duplicates from different niches)
* Inform the students in advance of the analysis required for the results using R, and make sure they have all the data they need in an organised format

## 5. Supplementary data

[R environment](https://tony-7752.github.io/Research-method-2026-Tony/images/R-assignment-env.RData)

[R script](https://tony-7752.github.io/Research-method-2026-Tony/images/R-assignment.R)

[Metadata](https://tony-7752.github.io/Research-method-2026-Tony/images/Photophysiology_metadata.csv)

## References

1. [Boardman, N. K. (1977). Comparative photosynthesis of sun and shade plants. *Annual Review of Plant Physiology*, 28(1), 355-377.](https://doi.org/10.1146/annurev.pp.28.060177.002035)

2. Falkowski, P. G., & Raven, J. A. (2013). *Aquatic Photosynthesis* (2nd ed.). Princeton University Press.

3. [Johansson, G., & Snoeijs, P. (2002). Macroalgal photosynthetic responses to light in relation to thallus morphology and depth zonation. *Marine Ecology Progress Series*, 244, 63-72.](https://www.int-res.com/journals/meps/articles/meps244063)

4. [Ralph, P. J., & Gademann, R. (2005). Rapid light curves: A powerful tool to assess photosynthetic activity. *Aquatic Botany*, 82(3), 222-237.](https://doi.org/10.1016/j.aquabot.2005.02.006)








