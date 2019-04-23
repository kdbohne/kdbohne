---
layout: splash
author_profile: true

title: *name
excerpt: Graphics Programmer

header:
  overlay_image: assets/images/terrain_noon.png

feature_row:
  - url: /terrain.html
    image_path: assets/images/terrain_lod.png
    title: "Terrain Renderer"
    alt: "Terrain Renderer"
    btn_label: "Read More"
    btn_class: "btn--small btn--inverse"
  - url: /pt.html
    image_path: assets/images/pt_spheres.png
    title: "Path Tracer"
    alt: "Path Tracer"
    btn_label: "Read More"
    btn_class: "btn--small btn--inverse"
  - url: /klang.html
    image_path: assets/images/klang.png
    title: "K Programming Language"
    alt: "K Programming Language"
    btn_label: "Read More"
    btn_class: "btn--small btn--inverse"
---

{% comment %}
HACK: Override the default CSS to change the position of the header.
TODO: is there a better way of doing this?
{% endcomment %}
<style type="text/css">
.page__hero--overlay { background-position: 0 20%; }
</style>

Graphics programmer with five years of experience creating real-time simulations with C++ and
OpenGL. Main area of interest is physically based rendering. Side projects include ray tracing and
compiler design.

{% include feature_row %}
