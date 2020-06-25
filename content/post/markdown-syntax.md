---
aliases:
- migrate-from-jekyl
author: Hugo Authors
categories:
- themes
- syntax
date: "2019-12-23"
description: Geometric effect analysis of highway-rail grade crossing safety performance.
imagelink: https://giskeramati.netlify.app/hrgc.jpg

series:
- Themes Guide
tags: []
title: Highway-Rail Grade Crossing Safety
---
# Table of Contents
1. [Project Abstract](#Project_Abstract)
2. [Map Digitizing](#Map_Digitizing)
3. [Nearest Intersection & Crossing](#Nearest_Intersection_&_Crossing_Distance)

<a name="Project_Abstract"></a>
## Project Abstract 
{{< highlight html >}}
Highway-rail grade crossings (HRGCs) are where a roadway and railway intersect at the
same level.Safety at HRGCs has been identified as a high-priority concern among
transportation agencies, but there has been little research on the effects of HRGC 
geometric parameters on their safety performance. This project evaluates the effects 
of HRGC geometric parameters on crash occurrence and severity likelihoods. The competing 
risk algorithm is selected to simultaneously analyze crash occurrence and severities. 
Four main HRGC geometric factors, along with other contributors, are investigated at
3,194 public HRGCs in North Dakota. This study focuses primarily on four geometric 
features of an HRGC: (1) acute crossing angle, (2) number of tracks (indicator of 
crossing width), (3) the roadway distance between the HRGC and the signalized 
intersection, (4) the railroad distance between the HRGC and the nearest crossing, and
(5) number of highway lanes. Distance to the nearest roadway intersections, nearest 
crossing and highway-railway crossing angles are map based calculations drawn from 
geographic information systems (GIS). The findings are: (1) all contributors tested 
in this study, including highway characteristics, traffic exposures from both railway 
and highway, and the four geometric features, significantly affect at least one crash 
severity level; (2) all contributors significantly impact crash frequency except for
the distance between crossings and the nearest roadway intersection; and (3) geometric 
parameters’ long-term effects on cumulative probability of crash severity and occurrence
over 30 years is also evaluated. Crossings with three main tracks contribute the highest
long-term crash probabilities.
{{< /highlight >}}

<a name="Map_Digitizing"></a>
## Map Digitizing 
First, North Dakota (ND) roadway network, railway network, signalized
intersections, and HRGCs from the ND GIS hub portal [NDHUB](https://gishubdata.nd.gov/)
are used in this study to estimate the numerical geometric measures of
each HRGC. All the GIS spatial features are adjusted to spatially align
and match with Google street features to obtain integrated and overlaid
coverage data before the geoprocessing calculation is performed. [Fig. 1](#Fig.1)
shows the integrated coverage map based on the five geometric measurements
calculation. The rail line, roadway, roadway intersections,
and HRGCs all align with the Google map of the area. Note that the
original NDHUB crossing locations are not spatially aligned with the
rest of the geo-features, but the spatial crossing locations were based on
aligned and matched digitized crossings.

<a name="Fig.1"></a>
Figure 1: Integrated and overlaid coverage with geometric measurements <a name="Fig.1"></a>
[<img src="/images/fig1array.png" alt="drawing" width="700"/>](/images/fig1array.png)

<a name="Nearest_Intersection_&_Crossing_Distance"></a>
## Nearest Intersection & Crossing 
Distances between crossings to their closest roadway intersection and crossing
are calculated by first identifying the nearest intersection/crossing to each
HRGC and then measuring the distance between the HRGC and the intersection/Crossing [[Fig. 2](#Fig.2)]. To estimate these distances GIS network analysis conducted by using the [ArcGIS Network Analyst extension](https://desktop.arcgis.com/en/arcmap/latest/extensions/network-analyst/what-is-network-analyst-.htm).

<a name="Fig.2"></a>
Figure 2: Finding the nearest road intersection and crossing along road and railroad <a name="Fig.2"></a>
[<img src="/images/IIFig2.jpg" alt="drawing" width="700"/>](/images/IIFig2.jpg)



The following HTML `<h1>`—`<h6>` elements represent six levels of section headings. `<h1>` is the highest section level while `<h6>` is the lowest.    

# H1
## H2
### H3
#### H4
##### H5
###### H6

## Paragraph

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Blockquotes

The blockquote element represents content that is quoted from another source, optionally with a citation which must be within a `footer` or `cite` element, and optionally with in-line changes such as annotations and abbreviations.

#### Blockquote without attribution

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use *Markdown syntax* within a blockquote.

#### Blockquote with attribution

> Don't communicate by sharing memory, share memory by communicating.</p>
> — <cite>Rob Pike[^1]</cite>


[^1]: The above quote is excerpted from Rob Pike's [talk](https://www.youtube.com/watch?v=PAAkCSZUG1c) during Gopherfest, November 18, 2015.

## Tables

Tables aren't part of the core Markdown spec, but Hugo supports supports them out-of-the-box.

   Name | Age
--------|------
    Bob | 27
  Alice | 23

#### Inline Markdown within tables

| Inline&nbsp;&nbsp;&nbsp;     | Markdown&nbsp;&nbsp;&nbsp;  | In&nbsp;&nbsp;&nbsp;                | Table      |
| ---------- | --------- | ----------------- | ---------- |
| *italics*  | **bold**  | ~~strikethrough~~&nbsp;&nbsp;&nbsp; | `code`     |

## Code Blocks

#### Code block with backticks

```
html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```
#### Code block indented with four spaces

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

#### Code block with Hugo's internal highlight shortcode
{{< highlight html >}}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
{{< /highlight >}}

## List Types

#### Ordered List

1. First item
2. Second item
3. Third item

#### Unordered List

* List item
* Another item
* And another item
    * sadfasdf

#### Nested list

* Item
1. First Sub-item
2. Second Sub-item

## Other Elements — abbr, sub, sup, kbd, mark

<abbr title="Graphics Interchange Format">GIF</abbr> is a bitmap image format.

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

Press <kbd><kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>Delete</kbd></kbd> to end the session.

Most <mark>salamanders</mark> are nocturnal, and hunt for insects, worms, and other small creatures.

