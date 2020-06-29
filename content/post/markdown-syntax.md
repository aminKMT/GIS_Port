---
aliases:
- migrate-from-jekyl
author: Amin Keramati
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
4. [Smallest Crossing Angle](#Smallest_Crossing_Angle)
5. [Computational Results](#Computational_Results)
6. [HRGC Geometric Effect Calculator](https://kmtgis.shinyapps.io/ak_plot/)
7. [Related Publication](https://doi.org/10.1016/j.aap.2020.105470)

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
[<img src="/images/fig1array.png" alt="drawing" title="Klick to see Larger scale!" width="700"/>](/images/fig1array.png)

<a name="Nearest_Intersection_&_Crossing_Distance"></a>
## Nearest Intersection & Crossing 
Distances between crossings to their closest roadway intersection and crossing
are calculated by first identifying the nearest intersection/crossing to each
HRGC and then measuring the distance between the HRGC and the intersection/Crossing [[Fig. 2](#Fig.2)]. To estimate these distances, GIS network analysis conducted by using the [ArcGIS Network Analyst extension](https://desktop.arcgis.com/en/arcmap/latest/extensions/network-analyst/what-is-network-analyst-.htm).

<a name="Fig.2"></a>
Figure 2: Finding the nearest road intersection and crossing along road and railroad <a name="Fig.2"></a>
[<img src="/images/IIFig2.jpg" alt="drawing" title="Klick to see Larger scale!" width="700"/>](/images/IIFig2.jpg)

<a name="Smallest_Crossing_Angle"></a>
## Smallest Crossing Angle
Crossing angle is one of the main factors impacting sight distance at a
crossing is its highway-railway angle.However, the national HRGC inventory data only report categorical
values for this variable.The calculation of acute crossing angles for
all crossings is described in [Fig.3](#Fig.3). It involves three steps: 1) create a
one-meter buffer around a crossing, 2) calculate the geo-coordinates of
road/rail intersections within the buffer, and 3) calculate the acute
angle based on all the coordinates. Acute angle is calculated based on
[Eq.1](#E1).

<a name="Fig.3"></a>
Figure 3: Crossing angle geometric factor measurements at HRGC. 
[<img src="/images/angdraw.png" alt="drawing" title="Klick to see Larger scale!" width="440"/>](/images/angdraw.png)<img src="/images/Degree_Zoom.png" alt="drawing" width="270"/>


### Equation 1: Estimating Crossing Angle <a name="E1"></a>
<img src="/images/Eq.jpg" alt="drawing" width="400"/> 

* Where:
* d<sub>x</sub>  is difference between x-coordinates of crossing and buffer intersection with railway
* d<sub>y</sub> is difference between y-coordinates of crossing and buffer intersection with railway
*	d<sup>a</sup><sub>x</sub> is difference between x-coordinates of crossing and buffer intersection with roadway
* d<sup>a</sup><sub>y</sub>  is difference between y-coordinates of crossing and buffer intersection with roadway
* r and r<sub>a</sub> are distances between crossing to railway-buffer and roadway-buffer intersections, respectively

##### 3,194 crossings' smallest angle were estimated through following python code based on [Eq.1](#E1) & [Fig.3](#Fig.3): 
{{< highlight html >}}
dx = !x1!-!x0!
dy = !y1!-!y0!
dxa = !x1a!-!x0a!
dya = !y1a!-!y0a!
r = math.sqrt(math.pow(dx,2) + math.pow(dy,2))
ra = math.sqrt(math.pow(dxa,2) + math.pow(dya,2))
Theta = math.asin(abs((dx*dya - dy*dxa))/(r*ra)) / math.pi * 180
{{< /highlight >}}

<a name="Computational_Results"></a>
## Computational Results
In the following section, we focus on the four geometric factors to
perform the cumulative probability analysis. To estimate marginal cumulative
probabilities for each geometric factor in 2018, the cumulative
probability of a target variable is estimated at a specific value while
all the other contributors are controlled at a fixed level, mode value.
The value range for each target geometric factor is selected from the
actual data used in this research. Distance between a crossing and the
nearest intersection ranges from 0 to 3,000 m (0–9,842 feet). Crossing
angle ranges from 1 to 90°. Number of roadway lanes ranges from 1 to
4. And number of main tracks ranges from 1 to 3. Each estimated cumulative
probability and trends for each target contributor are displayed
in [Fig.4](#Fig.4).

<a name="Fig.4"></a>
Figure 3: Cumulative crash/severity probabilities in 2018 for the four geometric contributors. 
[<img src="/images/a.png" alt="drawing" title="Crossing to Intersection Distance (Klick to see larger plot!)" width="360"/>](/images/a.png) [<img src="/images/b.png" alt="drawing" title="Crossing Angle (Klick to see larger plot!)" width="360"/>](/images/b.png) [<img src="/images/c.png" alt="drawing" title="Klick to see Larger scale!" width="360"/>](/images/c.png) [<img src="/images/d.png" alt="drawing" title="Klick to see Larger scale!" width="360"/>](/images/d.png)



