---
layout: page
title: seismic risk & hazard analysis 
description: feasibility study for seismic retrofit, [cee288] - final project
img: assets/img/p1_cover.png
importance: 1
category: structural
related_publications: false
toc:
  sidebar: left
---

Assessment of an on-campus building using HAZUS, SP3, and PBE at ground motion amplitudes with 2%, 10%, and 50% exceedance probabilities in 50 years. Consequence predictions were evaluated across analysis tools, and how they vary as a function of ground motion intensity.

<section>
<h3>Executive Summary</h3>
<p>Blackwelder High-rise is a 12-story graduate dormitory built in 1971
(<em>68 258 ft<sup>2</sup>, 146 ft tall</em>). A 2003 retrofit added interior concrete
shear walls.  Using <strong>HAZUS</strong>, <strong>SP3</strong> and <strong>PBE</strong>, YTVT Corporation
assessed current seismic risk and the cost-effectiveness of another
retrofit.  At 10 % exceedance in 50 years, the present-day expected loss
is \$4.93 million; a retrofit would lower it to \$4.80 million, a
difference too small to pay back the retrofit cost quickly.  Detailed
results, assumptions and recommendations follow.</p>
</section>

---

<section>
<h3>1  Building Description</h3>
The following address was used to perform analyses on the Blackwelder building:
781 Escondido Road, Stanford, CA 94305

The following building details and assumptions were used for all analyses:
<ol>
  <li>Address: 781 Escondido Rd, Stanford CA 94305</li>
  <li>Soil class: C</li>
  <li>Height: 146 ft</li>
  <li>Replacement cost: \$800 / ft<sup>2</sup></li>
  <li>Total area: 68 258 ft<sup>2</sup></li>
</ol>
</section>

<section>
<h3>2  Methodology</h3>

<h4>2.1  HAZUS</h4>
<p>Key parameters were taken from the HAZUS-MH MR4 technical manual:
building type <code>C2H</code>, occupancy <code>RES3a-f</code>.  Fragility curves were
developed for structural, acceleration-sensitive and drift-sensitive
components using the values in <a href="#p1_table1">Table 1</a>.  PGA hazard data
were taken from the Stanford PSHA 2013 report.</p>

<div id="p1_fig1" class="row">
  <div class="col-sm mt-3">
   <div style="width: 60%; margin: 0 auto;">
    {% include figure.liquid loading="eager" path="assets/img/p1_1.png"
       title="Hazard curve for site (USGS)" class="img-fluid rounded z-depth-1" %}
       </div>
  </div>
</div>
<div class="caption">Figure 1. Hazard curve for the Blackwelder site.</div>

Given these assumptions, the equivalent-PGA fragility curve parameters shown in <a href="#p1_table1">Table 1</a> were obtained from Table 5.16a of HAZUS®MH MR4 Technical Manual in order to properly reflect the seismic design of the structure. To evaluate the vulnerability of the Blackwelder building, fragility functions were produced using the provided parameters. Using the and values defined for each damage state and component type as shown in <a href="#p1_table1">Table 1</a>, four fragility functions were each plotted for structural, acceleration-sensitive, and drift-sensitive components as shown in <a href="#p1_fig2">Figure 2</a>. Note for
drift-sensitive components, the fragility functions were based on spectral displacement values due to the distribution parameters provided in the HAZUS manual.

<table id="p1_table1" align="center">
  <caption>Table 1. Fragility parameters for <code>C2H</code> building type.</caption>
  <thead><tr><th rowspan="2">Component</th><th colspan="2">DS1</th><th colspan="2">DS2</th><th colspan="2">DS3</th><th colspan="2">DS4</th></tr>
  <tr><th>θ</th><th>β</th><th>θ</th><th>β</th><th>θ</th><th>β</th><th>θ</th><th>β</th></tr></thead>
  <tbody>
    <tr><td>Structural</td><td>0.12</td><td>0.64</td><td>0.29</td><td>0.64</td><td>0.82</td><td>0.64</td><td>1.87</td><td>0.64</td></tr>
    <tr><td>Accel-sens.</td><td>0.30</td><td>0.68</td><td>0.60</td><td>0.66</td><td>1.20</td><td>0.65</td><td>2.40</td><td>0.65</td></tr>
    <tr><td>Drift-sens.</td><td>3.46</td><td>0.71</td><td>6.91</td><td>0.72</td><td>21.60</td><td>0.74</td><td>43.20</td><td>0.85</td></tr>
  </tbody>
</table>

<div id="p1_fig2" class="row">
  <div class="col-sm mt-3">
    {% include figure.liquid loading="lazy" path="assets/img/p1_2.png"
       title="Fragility functions by component &amp; damage state"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">Figure 2. Plot of fragility functions for each component type, separated by damage state.</div>

As expected, the fragility functions shift increasingly to the right of the plots as the damage states change from slight, moderate, extensive, to complete, indicating that the probability of exceeding that limit decreases with higher levels of damage. To then compute the probability of being in each damage state, we effectively take the difference between each of the fragility functions.

<a href="#p1_fig3">Figure 3</a> visualizes this data for each component type on a range of PGA values.

<div id="p1_fig3" class="row">
  <div class="col-sm mt-3">
    {% include figure.liquid loading="lazy" path="assets/img/p1_3.png"
       title="Plot of proability of damage by component &amp; damage state"
       class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">Figure 3. Plot of P(DS = DS | PGA) by PGA values for structural (left), acceleration-sensitive (middle), and drift-sensitive (right) components.</div>

<a href="#p1_table2">Table 2</a> summarizes the numbers plotted in <a href="#p1_fig3">Figure 3</a>. We can see clearly here that across each of the damage states, for each component type, the probabilities always decrease. Within each damage state, the probabilities increase as the % exceedance in 50 years also increases with DS1, but the same is not true for DS2-DS4.

<table id="p1_table2" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 2:</strong> Summary of probabilities of being in each DS, separated by component type.</caption>
  <thead>
    <tr>
      <th>Exceedance Probability</th>
      <th>Component</th>
      <th>DS1 (Slight)</th>
      <th>DS2 (Moderate)</th>
      <th>DS3 (Extensive)</th>
      <th>DS4 (Complete)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3">2 % (1.04 g)</td>
      <td>Structural</td>
      <td>0.0227</td>
      <td>0.3323</td>
      <td>0.4650</td>
      <td>0.1797</td>
    </tr>
    <tr>
      <td>Acceleration</td>
      <td>0.1687</td>
      <td>0.3847</td>
      <td>0.3136</td>
      <td>0.0992</td>
    </tr>
    <tr>
      <td>Drift</td>
      <td>0.2307</td>
      <td>0.5501</td>
      <td>0.1104</td>
      <td>0.0447</td>
    </tr>
    <tr>
      <td rowspan="3">10 % (0.55 g)</td>
      <td>Structural</td>
      <td>0.1500</td>
      <td>0.5751</td>
      <td>0.2384</td>
      <td>0.0279</td>
    </tr>
    <tr>
      <td>Acceleration</td>
      <td>0.3661</td>
      <td>0.3325</td>
      <td>0.1033</td>
      <td>0.0117</td>
    </tr>
    <tr>
      <td>Drift</td>
      <td>0.3688</td>
      <td>0.3347</td>
      <td>0.0232</td>
      <td>0.0072</td>
    </tr>
    <tr>
      <td rowspan="3">50 % (0.20 g)</td>
      <td>Structural</td>
      <td>0.5068</td>
      <td>0.2670</td>
      <td>0.0135</td>
      <td>0.0002</td>
    </tr>
    <tr>
      <td>Acceleration</td>
      <td>0.2275</td>
      <td>0.0451</td>
      <td>0.0029</td>
      <td>0.0001</td>
    </tr>
    <tr>
      <td>Drift</td>
      <td>0.1717</td>
      <td>0.0395</td>
      <td>0.0005</td>
      <td>0.0001</td>
    </tr>
  </tbody>
</table>

Using Tables 15.2-15.4 from the HAZUS Manual, the repair cost ratios are summarized in <a href="#p1_table3">Table 3</a>. These values were needed in order to calculate the expected cost of damage for different component types.

<!-- Table 3 -->
<table id="p1_table3" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 3:</strong> Repair Cost Ratios for each component type and damage state, in % of building replacement cost.</caption>
  <thead>
    <tr>
      <th>Component Type</th>
      <th>DS1 (Slight)</th>
      <th>DS2 (Moderate)</th>
      <th>DS3 (Extensive)</th>
      <th>DS4 (Complete)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Structural</td>
      <td>0.3</td>
      <td>1.4</td>
      <td>6.9</td>
      <td>13.8</td>
    </tr>
    <tr>
      <td>Acceleration-Sensitive<br>(Non-Structural)</td>
      <td>0.8</td>
      <td>4.3</td>
      <td>13.1</td>
      <td>43.7</td>
    </tr>
    <tr>
      <td>Drift-Sensitive<br>(Non-Structural)</td>
      <td>0.9</td>
      <td>4.3</td>
      <td>21.3</td>
      <td>42.5</td>
    </tr>
  </tbody>
</table>

To evaluate the impact of a seismic retrofit, the fragility functions were shifted by changing the median and the exceedance probabilities and the loss were recalculated. As a point of reference, the fragility curve median of a similar but more resilient HAZUS building type was used as shown in <a href="#p1_table4">Table 4</a>. The type selected was Steel Frame with Cast-in-Place Concrete Shear Walls (S4H) due to the presence of
<!-- Table 4 -->
<table id="p1_table4" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 4:</strong> Fragility Curve Parameters for S4H.</caption>
  <thead>
    <tr>
      <th>FORMAT<br>Theta<br>Beta</th>
      <th colspan="4">Damage State</th>
    </tr>
    <tr>
      <th>Component Type</th>
      <th>DS1 (Slight)</th>
      <th>DS2 (Moderate)</th>
      <th>DS3 (Extensive)</th>
      <th>DS4 (Complete)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Structural</td>
      <td>0.13<br>0.64</td>
      <td>0.25<br>0.64</td>
      <td>0.69<br>0.64</td>
      <td>1.63<br>0.64</td>
    </tr>
    <tr>
      <td>Acceleration-Sensitive<br>(Non-Structural)</td>
      <td>0.30<br>0.67</td>
      <td>0.60<br>0.65</td>
      <td>1.20<br>0.65</td>
      <td>2.40<br>0.65</td>
    </tr>
    <tr>
      <td>Drift-Sensitive<br>(Non-Structural)</td>
      <td>4.49<br>0.73</td>
      <td>8.99<br>0.73</td>
      <td>28.08<br>0.77</td>
      <td>56.16<br>0.89</td>
    </tr>
  </tbody>
</table>

<h4>2.2 SP3</h4>
The initial assumptions taken to build the SP3 model are listed as shown: 

1. The building was analyzed under Risk Category I/II. Seismic Importance Factor (Ie) of 1.00 as designated with a Drift Limit of 0.625 in both directions.
2. Ground Motion Hazard Information was sourced from the U.S. Geological Survey.
3. Building components classified as “Contents” such as desktop electronics, bookcases, etc. were left out of this analysis. Concrete link beams were also not included to provide the closest analog to the PBE model, which excludes this because the beam rotation EDP is not available (Please see Assumption 3 in Section 2.3).
4. There is some uncertainty within the quantity of objects inputted for SP3. Namely, the block number was taken as the number for quantity for each of the added components However, it may be possible in some cases that one block could indicate a proportion or multiplier for the real value of how many instances of a certain component exist.

With these assumptions and some preliminary inputs from the user including model and site information, site coordinates, and primary building information, the SP3 software was able to automate reports for risk analysis, detailed component damage breakdowns, and functional recovery. To name a few of the other changes made to the default model, a total of 2000 simulations were used, slender shear walls and RC Flat Slab were specified, an insulating glass unit glazing of 60% was approximated, and laterally braced metal stud partition walls with gypsum and wallpaper finishes were chosen to provide the most accurate match to the current state of the building. All building components used within the analysis were user-defined based on a given components spreadsheet in order to provide a more direct line of comparison to the PBE results, which uses the same component list. The default 1st Mode Period SA(\(T_1\)) of 1.91s in both directions was applied. The structure’s 1st-Mode Period, with 2%, 10%, and 50% exceedance probabilities in 50 years are listed in <a href="#p1_table5">Table 5</a>.

<!-- Table 5 -->
<table id="p1_table5" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 5:</strong> Spectral Acceleration Values corresponding to the 1st-Mode Period of the building at various exceedance rates.</caption>
  <thead>
    <tr>
      <th>S<sub>a</sub> (T<sub>1</sub> = 1.91 s)</th>
      <th>2 % in 50 Years</th>
      <th>10 % in 50 Years</th>
      <th>50 % in 50 Years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Direction 1</td>
      <td>0.618</td>
      <td>0.279</td>
      <td>0.076</td>
    </tr>
    <tr>
      <td>Direction 2</td>
      <td>0.628</td>
      <td>0.282</td>
      <td>0.077</td>
    </tr>
  </tbody>
</table>

<h4>2.3 PBE</h4>
The initial assumptions taken to build the PBE model are listed as shown:

1. A total of 9,125 worker days was specified for this analysis based on a 1 year repair timeframe with 25 workers. This number was computed by taking 365 days * 25 workers/day = 9,125 worker days.
2. All component assignments were taken from a pre-generated components spreadsheet.
3. Concrete link beams (Component ID: B.10.42) are not included because no EPD exists for beam rotation.
4. The concrete wall fragility (Component ID: B.10.44) uses interstory vs. effective drift.
5. Nonlinear response history analyses were already performed prior to our evaluation and the necessary structural response estimates were obtained. This allowed us to run the model with 50 Engineering Demand Parameter (EDP) values for ground motion intensities with 2%, 10%, and 50% exceedance probability in 50 years (using 50 ground motions consistent with the seismic hazard).

Given the inputs and assumptions from above, the PBE software was run with varying peak interstory drift (PID) and peak floor acceleration (PFA) data corresponding to ground motion data with 2%, 10%, and 50% exceedance in 50 years. The input demand values were plotted along the building height. This is illustrated in <a href="#p1_fig4">Figure 4</a>. In order to perform a quick check on whether the data is reasonable, we could compare the results with historical data of similar buildings in the area. The relative magnitude of drift ratios should be within \(10^{-2}\) to \(10^{-3}\) which is consistent with what is shown below.

<div id="p1_fig4" div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_4.png" title="mean drift and acc" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 4. Mean Drift (Peak Interstory Drift, ratio) and Acceleration (Peak Floor Acceleration, g).
</div>

Note that the demands on the structure vary between PBE and SP3. In general, the PID values reported from SP3 are higher than those inputted in PBE. At 2% exceedance probability, the peak drift in PBE was around 0.02 whereas SP3 reports 0.42. At 10% exceedance probability, the peak drift in PBE was around 0.015 whereas SP3 reports 0.07. At 50% exceedance probability, the peak drift in PBE was around 0.0035 whereas SP3 reports 0, though the latter
was likely truncated due to formatting.

However, the reverse is true for PFA values; PBE PFA values were typically higher than those from SP3. The differences in median PFA are shown in <a href="#p1_fig5">Figure 5</a>. At lower stories, the PFA values are close between the two models, but the difference between the two models increases along the building height.
 
 <div id="p1_fig5" div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_5.png" title="pbe sp3 comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5. PBE/SP3 comparison of median peak floor acceleration values by story, each direction. 
</div>

Exact numerical results are omitted for this project. Additional details provided upon request.

<br>
<br>
<h3>3  Discussion & Recommendations</h3>
<h4>3.1 Comparison</h4>
Overall there is a large range in the mean repair cost and recovery time between the three approaches as shown in Tables <a href="#p1_table6">Table 6</a>-<a href="#p1_table7">Table 7</a>. 

SP3 mostly estimates greater reconstruction cost and time values than PBE. The mean repair cost from SP3 is nearly double that from PBE at the 10% exceedance probability. At 2% exceedance probability, the SP3 mean repair cost is nearly 500% larger than that calculated with PBE. Previously, it was noted that the peak interstory drift demands were higher in SP3 than in PBE. From the results section, the components identified as top loss contributors were those analyzed under drift demand. Therefore it is understandable that the SP3 repair cost is much higher than that of PBE at lower exceedance probabilities. The repair cost from HAZUS is the highest between the models at the 10% and 50% exceedance probabilities. This is most likely due to the limitations associated with the HAZUS model (discussed in Section 4.2) and the fact that it performs the analyses with a general categorization of building type and therefore is not specific to Blackwelder in particular.

<table id="p1_table6" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 14:</strong> Mean repair cost [$] metric between the 3 models.</caption>
  <thead>
    <tr>
      <th>Model</th>
      <th>2 % Exceedance, 2475 years</th>
      <th>10 % Exceedance, 475 years</th>
      <th>50 % Exceedance, 72 years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>HAZUS</td>
      <td>12,677,019</td>
      <td>4,934,966</td>
      <td>752,568</td>
    </tr>
    <tr>
      <td>SP3</td>
      <td>27,300,000</td>
      <td>4,020,000</td>
      <td>268,000</td>
    </tr>
    <tr>
      <td>PBE</td>
      <td>4,600,890</td>
      <td>2,173,850</td>
      <td>344,113</td>
    </tr>
  </tbody>
</table>

<!-- Table 7 -->
<table id="p1_table7" align="center" border="1" cellpadding="4" cellspacing="0">
  <caption><strong>Table 15:</strong> Recovery time metric between the 3 models.</caption>
  <thead>
    <tr>
      <th>Model</th>
      <th>2 % Exceedance, 2475 years</th>
      <th>10 % Exceedance, 475 years</th>
      <th>50 % Exceedance, 72 years</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>HAZUS</td>
      <td>–</td>
      <td>–</td>
      <td>–</td>
    </tr>
    <tr>
      <td>SP3</td>
      <td>13 months</td>
      <td>9.5 months</td>
      <td>5.1 months</td>
    </tr>
    <tr>
      <td>PBE</td>
      <td>146 – 280 days<br>5 – 9 months</td>
      <td>17 – 87 days<br>0.56 – 2.9 months</td>
      <td>5 – 23 days<br>0.17 – 0.77 months</td>
    </tr>
  </tbody>
</table> 

<section> 
	<h4><br>3.2  Limitations</h4>
	HAZUS is a generalized approach to loss estimation which relies on an inventory of buildings grouped by type. The loss distribution is based on the average of the building type, and therefore it does not consider site specific components. As specified in the HAZUS manual, buildings within the same category can experience very different damage and losses in an earthquake event, which means the calculated loss is a very rough estimate. An additional limitation noted in the manual is that predicted ground motions at areas very close to faults in California, like the Stanford site, may be inconsistent due to approximate modeling. Also, the method in which HAZUS computes loss ratio does not consider any uncertainty, a fact that may have resulted in a higher percentage of error for that calculation. 

In this study, the inputs to the PBE software may have limited the results. Though the program is able to perform structural analysis, this option was not used in the interest of time. 50 ground motions were used to find engineering demand parameters used as input. This step was performed by a third party and was not thoroughly reviewed. Additionally, no distribution was assigned for repair cost per square foot and worker labor days, so uncertainty in these numbers was not considered. 

As a commercial software, SP3 makes a number of assumptions for ease of use. There were many default values such as drift limit, plan irregularities, base shear strength, and mode periods, all of which were accepted in the analysis due to lack of building information. 

In addition to the software-specific limitations, there are some potential drawbacks to the FEMA P-58 methodology itself. For instance, considering the fact that FEMA P-58 requires inputs about building data, it can require the user to make assumptions about multiple characteristics if there is not sufficient information given. Also, even if specific components of the seismic analysis are verified, the model ultimately needs to be compared to real-world events to ensure that we are receiving accurate results from the simulated data.
</section>

<section>
	<h4><br>4  Conclusion</h4>
	Given the results of the seismic evaluation, it is clear that retrofitting the building would result in an overall smaller total expected loss and AAL (average annual loss), but not to the extent that could fully justify the cost of the retrofit itself. More specifically, without the retrofit option, the total expected loss for PGA (Peak Ground Acceleration) levels with 10% exceedance probability in 50 years is $4,934,966 whereas the same metric post-retrofit would be $4,802,830. In comparing this metric alone, proceeding with the retrofit option would theoretically save $132,136, an amount that is not significant in the overall scheme of building construction. Even more, the payback period would not be realistically feasible due to the upfront cost to retrofit the building in the first place. There is a 1.2% - 5.2% decrease when comparing the AAL of the original building to that of the retrofit option, with the 2% exceedance probability in 50 years having the largest percent decrease. 

While these numerical values from the HAZUS model are insightful, we acknowledge that there are consequential restrictions associated with relying solely on the approach taken by this model. To mitigate the error that may result from these numbers, more priority was given to the results generated from SP3 and PB3, both of which apply the performance-based earthquake engineering (PBEE) methodology, which allows for more exhaustive analysis unique to the Blackwelder building itself. 

Our final recommendation is to leave the Blackwelder building as-is unless there is cause for greater concern with any of the structural components, and only proceed with the retrofit if the university is already considering a tangentially-related project such as a revamp. In doing so, the university would not have to incur any substantial additional costs because the structural membrane would already be exposed, allowing for an efficient retrofit process that would enhance the building resiliency.
</section>

<br>This project was completed with the following person(s): YHT

<br>
<br>

<section>
<h3>References</h3>
<ol>
1. Applied Technology Council. 2015a. <em>FEMA P-154: Rapid Visual Screening of Buildings for Potential Seismic Hazards: A Handbook.</em><br>
2. Applied Technology Council. 2015b. <em>FEMA P-155: Rapid Visual Screening of Buildings for Potential Seismic Hazards: Supporting Documentation.</em><br>
3. Applied Technology Council. 2018. <em>FEMA P-58: Seismic Per<strong>f</strong>ormance Assessment of Buildings.</em><br>
4. International Conference of Building Officials. 1967. <em>Uniform Building Code 1967 Edition.</em><br>
</ol>
</section>
