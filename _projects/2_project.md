---
layout: page
title: failure of 2002 rupture of enbridge pipeline 
description: case study
img: assets/img/3.jpg
importance: 2
category: past
giscus_comments: true
---

In this project, 3 primary modes of failure (fracture, corrosion, fatigue) are studied to assess the causes of the Enbridge oil pipeline rupture.

Assuming linear elastic fracture mechanics, a hoop stress and critical flaw size was determined and then scaled to take into account the reduced K1c
of the weld. This critical crack length was then used in further analysis for fatigue failure. Since the SSY condition was not appropriately satisfied, this calculated critical crack size was marked as a lower-bound estimate. Because of this nuance, a brief dive into non-linear fracture mechanics was also explored, making use of the energy release perspective and Griffith’s Fracture Criterion. Under this investigation, a rough estimation for the process zone was found making use of the LEFM approach where σ∞ < σy. The crack propagation was proven to be unstable with the ∂G/∂a > 0 condition. In assessing the impact of corrosion on the pipeline, signs of iron sulfide pointed to hydrogen embrittlement. The chemical reactions for this process were detailed and the cell potential ∆G was found–the corresponding positive cell potential and negative ∆G demonstrated how this type of corrosion was spontaneous. While this may have contributed to exacerbating the failure of
the pipe, there was not enough evidence to indicate any serious pitting. To get a deeper understanding of the mechanisms for fatigue in this situation, Paris’ Law
was employed to find the number of cycles to failure against three different types of steel: martensitic, ferrite-pearlite, and ausetnitic stainless. Between the three, martensitic resulted in the least number of cycles at 3,882,909.85 while ferrite-pearlite resulted in the highest number at 12,598,849.81 cycles with a ∆σ = 26.5 ksi for each cycle. The large diameter-thickness ratio of the pipe (109:1) most likely also contributed to its high susceptibility to cyclic stresses during transportation and loading. Even more, welded sections are generally known to be more liable to fatigue cracks.

Potential sources of uncertainty within our analysis could’ve stemmed from the assumed crack shape, the variation in stress during the fatigue cycle, and the material properties of C and m. While it was noted that the specific steel used for the construction of this pipeline was 5L grade X52, only the C and m values of martensitic, ferrite-pearlite, and austenitic stainless were given. In addition, if more information was given about the behavior of
the pipeline flow, further examination could’ve expanded upon the impact of flow type (laminar or turbulent) on applied forces within the wall. Further work could also include a fatigue analysis using the S-N curve to validate our findings with Paris’ Law and reduce variability in regards to the triaxiality of the section.

<!--
</section>
    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images.
Say you wanted to write a little bit about your project before you posted the rest of the images.
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
-->
