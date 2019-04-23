---
layout: single
author_profile: true
permalink: /pt

classes: wide

title: Path Tracer
excerpt: Personal implementation of pbrt

header:
  overlay_image: /assets/images/pt_spheres.png
---

<style type="text/css">
.page__hero--overlay { height: 600px; background-position: 0 40%; }
</style>

<div class="no_margin_top">
<h2>Overview</h2>
</div>

[pt](https://github.com/kdbohne/pt) is an offline path tracer based on
[pbrt-v3](https://github.com/mmp/pbrt-v3), the source code provided with the
third edition of *Physically Based Rendering: From Theory to Implementation*
by Matt Phar, Wenzel Jakob, and Greg Humphreys.

This path tracer implements a subset of the functionality provided by pbrt
for the purpose of learning the intricacies of a photorealistic renderer.

## Examples

The [pbrt-v3-scenes](https://pbrt.org/scenes-v3.html) repository provides many example scenes in
the [pbrt](https://pbrt.org/fileformat-v3.html) file format. Below are a few of these scenes
run through `pt`.

<a href="/assets/images/pt_spheres.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/pt_spheres.png)
</a>
<figcaption>pbrt-v3-scenes/figures/f11-15.pbrt</figcaption>

<br>

<a href="/assets/images/pt_lights.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/pt_lights.png)
</a>
<figcaption>pbrt-v3-scenes/veach-mis/mis.pbrt</figcaption>
