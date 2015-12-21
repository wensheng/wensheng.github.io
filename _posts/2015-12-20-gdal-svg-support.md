---
layout: post
title: Gdal SVG support
---

I wanted to convert a SVG file to GeoJSON.  [Gdal](http://www.gdal.org/) command ogr2ogr is supposed to support these 2 formats.  But when I tried it:

    $ogr2ogr -f GeoJSON circle.json circle.svg
    FAILURE:
    Unable to open datasource `circle.svg' with the following drivers.

It then listed drivers that includes both geojson and svg. What's going on?

Turns out that gdal only support a special kind of SVG, which is in the format of "Cloudmade Vector Stream Server" svg output. (http://www.gdal.org/drv_svg.html)

Cloudmade, unfortunately no longer support individual developer and had withdrew documentations from its website, so there's no way to know what this special SVG looks like.

I guess writing a SVG to GeoJSON converter shouldn't be too difficult, I might give it a try.
