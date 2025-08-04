---
layout: page
title: cee288 final project
description: feasibility study for seismic retrofit 
img: assets/img/12.jpg
importance: 1
category: past
related_publications: true
---

assessment of an on-campus building using HAZUS, SP3, and PBE at ground motion amplitudes with 2%, 10%, and 50% exceedance probabilities in 50 years. consequence predictions were evaluated across analysis tools, and how they vary as a function of ground motion intensity.

<section>
  <h3>Building Description</h3>
  <p>The following address was used to perform analyses on the Blackwelder building:
      781 Escondido Road, Stanford, CA 94305 </p>
   <p>The following building details and assumptions were used for all analyses: </p>
    <ol>
        <li>Soil Information: Site Class C</li>
        <li>Building Height: 146 feet</li>
        <li>Total Cost per Square Foot = $800/sf</li>
        <li>Total Building Square Footage = 68,258.43sf</li>
    </ol>  
</section>

<section>
 <u>Assumptions</u> 
  <ol>
        <li> In order to analyze the current Blackwelder building, we defined the following parameters using Table 3.1 and 3.2 from the HAZUS®MH MR4 Technical Manual:
              a.	Building Type: C2H (High Rise Concrete Shear Wall)
              b.	Occupancy Type: Multi-Family Dwelling (RES3a-f) </li>
      <li>For the building retrofit option, new analysis was modeled with the following building type:
              a.	Building Type: S4H (Steel Frame with Cast-in-Place Concrete Shear Walls)</li>
      <li> Exceedance probabilities at each ground motion were taken from the Stanford PSHA 2013 Report for Site 1 conducted by the URS Corporation because more discretization of the hazard curve was desired. From the report, a step of 0.05g was recorded compared to the available USGS data shown in Figure 1.</li>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_1.png" title="hazard curve" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 1. Hazard curve for site from USGS.
</div>
    
<section>
	
  <h4>HAZUS</h4>
  To evaluate the vulnerability of the Blackwelder building, fragility functions were produced using Eqn. (1) below. Fragility functions are probability distributions used   to describe how likely a component is to be damaged within a damage state when subjected to a specific ground motion. In our study, fragility functions take the form of a lognormal cumulative distribution function as follows:

  \begin{align}
    P(DS ≥ ds | S_a) &= \phi \left(\dfrac{1}{\beta_{ds}} ln \left(\dfrac{S_a}{\theta_{ds}} \right) \right)
  \end{align}
</section>

where DS = Damage State, S_a  refers to the spectral acceleration values, and θ and β are the input parameters that correspond to median and standard deviation of the probability distribution. 

Using the θ and β values defined for each damage state and component type as shown in Table 1, four fragility functions were each plotted for structural, acceleration-sensitive, and drift-sensitive components as shown in Figure 2. Note for drift-sensitive components, the fragility functions were based on spectral displacement values due to the distribution parameters provided in the HAZUS manual.

<table>
  <caption>Table 1: Fragility Curve Parameters for the C2H building type.</caption>
  <thead>
  <tr>
    <th>θ<br>β</th>
    <th colspan="4">Damage State</th>
  </tr></thead>
<tbody>
  <tr>
    <td>Component Type</td>
    <td>DS1 <br>(Slight)</td>
    <td>DS2<br>(Moderate)</td>
    <td>DS3<br>(Extensive)</td>
    <td>DS4<br>(Complete)</td>
  </tr>
  <tr>
    <td>Structural </td>
    <td>&nbsp;&nbsp;&nbsp;<br>0.12<br>&nbsp;&nbsp;&nbsp;<br>0.64&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>0.29<br>&nbsp;&nbsp;&nbsp;<br>0.64&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>0.82<br>&nbsp;&nbsp;&nbsp;<br>0.64&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1.87<br>&nbsp;&nbsp;&nbsp;<br>0.64&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>Acceleration-Sensitive (Non-Structural)</td>
    <td>&nbsp;&nbsp;&nbsp;<br>0.30<br>&nbsp;&nbsp;&nbsp;<br>0.68&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>0.60<br>&nbsp;&nbsp;&nbsp;<br>0.66&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1.20<br>&nbsp;&nbsp;&nbsp;<br>0.65&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>2.40<br>&nbsp;&nbsp;&nbsp;<br>0.65&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>Drift-Sensitive (Non-Structural)</td>
    <td>&nbsp;&nbsp;&nbsp;<br>3.46<br>&nbsp;&nbsp;&nbsp;<br>0.71&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>6.91<br>&nbsp;&nbsp;&nbsp;<br>0.72&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>21.60<br>&nbsp;&nbsp;&nbsp;<br>0.74&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>43.20<br>&nbsp;&nbsp;&nbsp;<br>0.85&nbsp;&nbsp;&nbsp;</td>
  </tr>
</tbody></table>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_2.png" title="fragility func" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 2. Plot of fragility functions for each component type, separated by damage state.
</div>

As expected, the fragility functions shift increasingly to the right of the plots as the damage states change from slight, moderate, extensive, to complete, indicating that the probability of exceeding that limit decreases with higher levels of damage. To then compute the probability of being in each damage state, we effectively take the difference between each of the fragility functions as such:
  \begin{align}
      P(DS = ds_i  | S_a) = P(DS ≥ ds_i  | S_a) - P(DS ≥ ds_{i+1}  | S_a)
  \end{align}

Figure 3 visualizes this data for each component type on a range of PGA values.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_3.png" title="p_ds" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 3. Plot of P(DS = DS | PGA) by PGA values for structural (left), acceleration-sensitive (middle), and drift-sensitive (right) components. 
</div>

To evaluate the impact of a seismic retrofit, the fragility functions were shifted by changing the median and the exceedance probabilities and the loss were recalculated. As a point of reference, the fragility curve median of a similar but more resilient HAZUS building type was used. The type selected was Steel Frame with Cast-in-Place Concrete Shear Walls (S4H) due to the presence of shear walls and improved performance. 

<section>
  <h4><br>SP3</h4>
  The initial assumptions taken to build the SP3 model are listed as shown:
</section>
<section>
  <u>Assumptions</u>
   <ol>
        <li>The building was analyzed under Risk Category I/II. Seismic Importance Factor (Ie) of 1.00 as designated with a Drift Limit of 0.625 in both directions.
	Ground Motion Hazard Information was sourced from the U.S. Geological Survey.</li>
        <li>Building components classified as “Contents” such as desktop electronics, bookcases, etc. were left out of this analysis. Concrete link beams were also not included to provide the closest analog to the PBE model, which excludes this because the beam rotation EDP is not available.</li>
        <li>There is some uncertainty within the quantity of objects inputted for SP3. Namely, the block number was taken as the number for quantity for each of the added   components. However, it may be possible in some cases that one block could indicate a proportion or multiplier for the real value of how many instances of a certain component exist.</li>
    </ol>  

With these assumptions and some preliminary inputs from the user including model and site information, site coordinates, and primary building information, the SP3 software was able to automate reports for risk analysis, detailed component damage breakdowns, and functional recovery. To name a few of the other changes made to the default model, a total of 2000 simulations were used, slender shear walls and RC Flat Slab were specified, an insulating glass unit glazing of 60% was approximated, and laterally braced metal stud partition walls with gypsum and wallpaper finishes were chosen to provide the most accurate match to the current state of the building. All building components used within the analysis were user-defined based on a given components spreadsheet in order to provide a more direct line of comparison to the PBE results, which uses the same component list. The default 1st Mode Period SA(T_1) of 1.91s in both directions was applied. The structure’s 1st-Mode Period, with 2%, 10%, and 50% exceedance probabilities in 50 years are listed in Table 2.
</section>
	
<style type="text/css">
.tg  {border-collapse:collapse;border-color:#ccc;border-spacing:0;}
.tg td{background-color:#fff;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg th{background-color:#f0f0f0;border-color:#ccc;border-style:solid;border-width:1px;color:#333;
  font-family:Arial, sans-serif;font-size:14px;font-weight:normal;overflow:hidden;padding:10px 5px;word-break:normal;}
.tg .tg-ov6f{background-color:#ffffff;border-color:#333333;text-align:left;vertical-align:top}
.tg .tg-btxf{background-color:#f9f9f9;border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
</style>
<table class="tg" style="undefined;table-layout: fixed; width: 649px"><colgroup>
		<caption>Table 2: Spectral Acceleration Values corresponding to the 1st-Mode Period of the building at various exceedance rates.</caption>
<col style="width: 141.333333px">
<col style="width: 160.333333px">
<col style="width: 174.333333px">
<col style="width: 173.333333px">
</colgroup>
<thead>
  <tr>
    <th class="tg-ov6f">Sa (T_1 = 1.91s)</th>
    <th class="tg-ov6f">2% in 50 Years</th>
    <th class="tg-ov6f"><span style="color:black">10% in 50 Years</span>   </th>
    <th class="tg-ov6f"> <span style="color:black">50% in 50 Years</span>   </th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-btxf">   Direction 1   </td>
    <td class="tg-btxf">   0.618   </td>
    <td class="tg-btxf">   0.279   </td>
    <td class="tg-btxf">   0.076   </td>
  </tr>
  <tr>
    <td class="tg-0pky">   Direction 2   </td>
    <td class="tg-0pky">   0.628   </td>
    <td class="tg-0pky">   0.282   </td>
    <td class="tg-0pky">   0.077   </td>
  </tr>
</tbody>
</table>

<section>
  <h4><br>PBE</h4>
  The initial assumptions taken to build the PBE model are listed as shown:
</section>

<section>
  <u>Assumptions</u>
    <ol>
        <li> A total of 9,125 worker days was specified for this analysis based on a 1 year repair timeframe with 25 workers. This number was computed by taking 365 days * 25 workers/day = 9,125 worker days. </li>
        <li>All component assignments were taken from a pre-generated components spreadsheet. </li>
        <li>Concrete link beams (Component ID: B.10.42) are not included because no EPD exists for beam rotation. </li>
        <li>The fragilities of the concrete wall (Component ID: B.10.44) uses interstory vs. effective drift.</li>
        <li>Nonlinear response history analyses were already performed prior to our evaluation and the necessary structural response estimates were obtained. This allowed us to run the model with 50 Engineering Demand Parameter (EDP) values for ground motion intensities with 2%, 10%, and 50% exceedance probability in 50 years (using 50 ground motions consistent with the seismic hazard).</li>

Given the inputs and assumptions from above, the PBE software was run with varying peak interstory drift (PID) and peak floor acceleration (PFA) data corresponding to ground motion data with 2%, 10%, and 50% exceedance in 50 years. The input demand values were plotted along the building height. This is illustrated in Figure 4, where each row provides the drift and acceleration plots for 2%, 10, and 50% exceedance probability in 50 years.

In order to perform a quick check on whether the data is reasonable, we could compare the results with historical data of similar buildings in the area. The relative magnitude of drift ratios should be within 10^-2 to 10^-3 which is consistent with what is shown below. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_4.png" title="mean drift and acc" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 4. Mean Drift (Peak Interstory Drift, ratio) and Acceleration (Peak Floor Acceleration, g).
</div>

Note that the demands on the structure vary between PBE and SP3. In general, the PID values reported from SP3 are higher than those inputted in PBE. At 2% exceedance probability, the peak drift in PBE was around 0.02 whereas SP3 reports 0.42. At 10% exceedance probability, the peak drift in PBE was around 0.015 whereas SP3 reports 0.07. At 50% exceedance probability, the peak drift in PBE was around 0.0035 whereas SP3 reports 0, though the latter was likely truncated due to formatting. 

However, the reverse is true for PFA values; PBE PFA values were typically higher than those from SP3. The differences in median PFA are shown in Figure 5. At lower stories, the PFA values are close between the two models, but the difference between the two models increases along the building height.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_5.png" title="pbe sp3 comparison" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Figure 5. PBE/SP3 comparison of median peak floor acceleration values by story, each direction. 
</div>

<br>Exact numerical results are omitted for this project. Additional details provided upon request. 

<section> 
	<h4><br>Limitations</h4>
	<br> HAZUS is a generalized approach to loss estimation which relies on an inventory of buildings grouped by type. The loss distribution is based on the average of the building type, and therefore it does not consider site specific components. As specified in the HAZUS manual, buildings within the same category can experience very different damage and losses in an earthquake event, which means the calculated loss is a very rough estimate. An additional limitation noted in the manual is that predicted ground motions at areas very close to faults in California, like the Stanford site, may be inconsistent due to approximate modeling. Also, the method in which HAZUS computes loss ratio does not consider any uncertainty, a fact that may have resulted in a higher percentage of error for that calculation. 
In this study, the inputs to the PBE software may have limited the results. Though the program is able to perform structural analysis, this option was not used in the interest of time. 50 ground motions were used to find engineering demand parameters used as input. This step was performed by a third party and was not thoroughly reviewed. Additionally, no distribution was assigned for repair cost per square foot and worker labor days, so uncertainty in these numbers was not considered. 
As a commercial software, SP3 makes a number of assumptions for ease of use. There were many default values such as drift limit, plan irregularities, base shear strength, and mode periods, all of which were accepted in the analysis due to lack of building information. 
In addition to the software-specific limitations, there are some potential drawbacks to the FEMA P-58 methodology itself. For instance, considering the fact that FEMA P-58 requires inputs about building data, it can require the user to make assumptions about multiple characteristics if there is not sufficient information given. Also, even if specific components of the seismic analysis are verified, the model ultimately needs to be compared to real-world events to ensure that we are receiving accurate results from the simulated data.
</section>

<section>
	<h4><br>Conclusion</h4>
	Given the results of the seismic evaluation, it is clear that retrofitting the building would result in an overall smaller total expected loss and AAL (average annual loss), but not to the extent that could fully justify the cost of the retrofit itself. More specifically, without the retrofit option, the total expected loss for PGA (Peak Ground Acceleration) levels with 10% exceedance probability in 50 years is $4,934,966 whereas the same metric post-retrofit would be $4,802,830. In comparing this metric alone, proceeding with the retrofit option would theoretically save $132,136, an amount that is not significant in the overall scheme of building construction. Even more, the payback period would not be realistically feasible due to the upfront cost to retrofit the building in the first place. There is a 1.2% - 5.2% decrease when comparing the AAL of the original building to that of the retrofit option, with the 2% exceedance probability in 50 years having the largest percent decrease. 

While these numerical values from the HAZUS model are insightful, we acknowledge that there are consequential restrictions associated with relying solely on the approach taken by this model. To mitigate the error that may result from these numbers, more priority was given to the results generated from SP3 and PB3, both of which apply the performance-based earthquake engineering (PBEE) methodology, which allows for more exhaustive analysis unique to the Blackwelder building itself. 

Our final recommendation is to leave the Blackwelder building as-is unless there is cause for greater concern with any of the structural components, and only proceed with the retrofit if the university is already considering a tangentially-related project such as a revamp. In doing so, the university would not have to incur any substantial additional costs because the structural membrane would already be exposed, allowing for an efficient retrofit process that would enhance the building resiliency.
</section>

<br>This project was completed with the following person(s): YHT
