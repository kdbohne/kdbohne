---
layout: single
author_profile: true
permalink: /terrain

classes: wide

title: Terrain Renderer
excerpt: Real-time renderer using C++ and OpenGL 3.3

header:
  overlay_image: /assets/images/terrain_noon.png
---

{% comment %}
HACK: Override the default CSS to change the height of the header.
TODO: is there a better way of doing this?
{% endcomment %}
<style type="text/css">
.page__hero--overlay { height: 600px; }
</style>

<div class="no_margin_top">
<h2>Overview</h2>
</div>

This is a real-time terrain renderer written using C++ and OpenGL 3.3. The terrain is rendered as a series
of static meshes (chunks) that are displaced in a vertex shader based on a heightmap. The chunks are
assembled into a quadtree that is traversed at runtime to provide dynamic level of detail.
Instanced meshes such as trees and rocks are scattered across the terrain and assigned to chunks.

## Level of Detail

The heightmap is divided into chunks of varying sizes during a preprocessing stage. These chunks are
then assembled into a [quadtree](https://en.wikipedia.org/wiki/Quadtree). At runtime, the quadtree
is subdivided from its root based on a simple distance heuristic. The result is a dynamic
[level of detail](https://en.wikipedia.org/wiki/Level_of_detail) optimization, visualized here:

<a href="assets/images/terrain_lod.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/terrain_lod.png)
</a>
<figcaption>Five different levels of detail visualized by color (green = highest LOD, blue = lowest LOD)</figcaption>

## Cascaded Shadow Maps

Before the forward lighting pass, the renderer generates a set of
[cascaded shadow maps](https://docs.microsoft.com/en-us/windows/desktop/dxtecharts/cascaded-shadow-maps)
for the visible scene. This provides high quality shadows at both near and far distances without
requiring an extremely large shadow map. The number of cascades is configurable, with the commonly
used four being shown here:

<a href="/assets/images/terrain_cascade.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/terrain_cascade.png)
</a>
<figcaption>Four shadow map cascades, decreasing in detail from left to right</figcaption>

## Atmospheric Scattering

The sky is rendered with a fullscreen quad that samples a set of precomputed textures. These
textures are generated based on the work of
[Precomputed Atmospheric Scattering](https://ebruneton.github.io/precomputed_atmospheric_scattering)
(Bruneton 2017). As a result, the renderer supports dynamic time of day for both the sky and for
ambient light contribution.

<a href="/assets/images/terrain_sunset.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/terrain_sunset.png)
</a>
<figcaption>Atmospheric scattering at sunset with visible Rayleigh and Mie scattering</figcaption>

## Physically Based Rendering

The renderer supports a [physically based](https://en.wikipedia.org/wiki/Physically_based_rendering)
material model. Each material is represented by four textures (or constants):
 * Albedo (base color)
 * Normal
 * Metallic
 * Roughness

The material is then shaded by combining the [diffuse Lambertian](https://en.wikipedia.org/wiki/Lambertian_reflectance)
and [specular Cook-Torrance](http://inst.cs.berkeley.edu/~cs294-13/fa09/lectures/cookpaper.pdf)
[BRDFs](https://en.wikipedia.org/wiki/Bidirectional_reflectance_distribution_function) based on the
incoming direct and indirect light.

<a href="/assets/images/terrain_pbr.png">
![no-alignment]({{ site.url }}{{ site.baseurl }}/assets/images/terrain_pbr.png)
</a>
<figcaption>Physically based materials (top row: dielectric; bottom row: metallic; roughness
increasing from left to right). Far right: test model and textures</figcaption>

## Additional Features
 * Cross-platform support (Windows, Linux)
 * [High dynamic range](https://en.wikipedia.org/wiki/High_dynamic_range) pipeline
    * [Reinhard](http://www.cmap.polytechnique.fr/~peyre/cours/x2005signal/hdr_photographic.pdf) and [filmic](http://filmicworlds.com/blog/filmic-tonemapping-operators) tone mapping
    * Adaptive camera exposure based on scene luminance
 * Custom mesh and scene formats with [Blender](https://www.blender.org) importer/exporter
 * Dynamic text rendering using [FreeType](https://www.freetype.org)
 * Sortable render queue to minimize state changes
