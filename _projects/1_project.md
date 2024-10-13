---
layout: page
title: CEE288 Final Project
description: feasibility study for seismic retrofit 
img: assets/img/12.jpg
importance: 1
category: work
related_publications: true
---

As the final project of CEE288 course at Stanford, students were asked to perform assessments of an on-campus building using HAZUS, SP3, and PBE at ground motion
amplitudes with 2%, 10%, and 50% exceedance probabilities in 50 years. Consequence predictions were evaluated across analysis tools, and how they vary as a function of ground motion intensity.

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

</section>
    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<section>
  <h4>HAZUS</h4>
  To evaluate the vulnerability of the Blackwelder building, fragility functions were produced using Eqn. (1) below. Fragility functions are probability distributions used   to describe how likely a component is to be damaged within a damage state when subjected to a specific ground motion. In our study, fragility functions take the form of a lognormal cumulative distribution function as follows:

  \begin{align}
    P(DS >= ds | S_a) &= \phi \left(\dfrac{1}{\beta_{ds}} ln \left(\dfrac{S_a}{\theta_{ds}} \right) \right)
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
    Plot of fragility functions for each component type, separated by damage state.
</div>

As expected, the fragility functions shift increasingly to the right of the plots as the damage states change from slight, moderate, extensive, to complete, indicating that the probability of exceeding that limit decreases with higher levels of damage. To then compute the probability of being in each damage state, we effectively take the difference between each of the fragility functions as such:
  \begin{align}
      P(DS = ds_i  | S_a) = P(DS >= ds_i  | S_a) - P(DS >= ds_{i+1}  | S_a)
  \end{align}

Figure 3 visualizes this data for each component type on a range of PGA values.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1_3.png" title="p_ds" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Plot of P(DS = DS | PGA) by PGA values for structural (left), acceleration-sensitive (middle), and drift-sensitive (right) components. 
</div>

To evaluate the impact of a seismic retrofit, the fragility functions were shifted by changing the median and the exceedance probabilities and the loss were recalculated. As a point of reference, the fragility curve median of a similar but more resilient HAZUS building type was used. The type selected was Steel Frame with Cast-in-Place Concrete Shear Walls (S4H) due to the presence of shear walls and improved performance. 


You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
