---
author: Amin Keramati
date: "2017-03-05"
description: Executing python script to allow map-matching of bike GPS points to a network in “ArcPy”.
imagelink:  https://giskeramati.netlify.app/Cover3.jpg 
tags2:
- emoji
title: Bike Route Choice Modeling Analysis
---

# Table of Contents
1. [Project Abstract](#Project_Abstract2)
2. [Data](#Data)
3. [Methodology](#Methodology)


<a name="Project_Abstract2"></a>
## Project Abstract 
{{< highlight html >}}

In this project, I executed several python scripts and GIS models to allow map-matching of 
more than 14,000 bike GPS points to the Downtown Toronto street network. These data are 
using as part of the project titled "Bike route choice modeling analysis". 

{{< /highlight >}}

<a name="Data"></a>
## Data  
The GPS points in this project indicate several number of specific routes which are passed by several number of bike riders. However, GPS points can have deviation usually because of satellite or GPS coordinate recording devices errors. In the bike transportation network analysis, map-matching technics are used to match the sets of GPS points indicating the bike rider's route to the real route that bike rider passes on. In this project the transportation network is Downtown Toronto street network [Fig.1(a)](#Fig.1), and 14,000 GPS points are recorded [Fig.1(b)](#Fig.1). As can be seen in [Fig.2](#Fig.2), GPS points attribute table has information like "START_AT", "END_AT", and TRIP_ID. START and END At indicates the GPS points recording time and TRIP_ID indicates the specific route that a set of GPS points indicates. In order to match the path with each set of GPS point, I first made a model to separate sets of point in to the separate point shapefile based on their TRIP_ID. The results were 4,389 GPS points set with unique TRIP_ID which indicats there are 4,389 routes to match. [Fig.3](#Fig.3) indicates the separated GPS points set in Arc Catalog. 

<a name="Fig.1"></a>
Figure 1: Downtown Toronto Street Network and GPS Points. 
[<img src="/images/Tnet.png" alt="drawing" title="a" width="340
"/>](/images/Tnet.png) [<img src="/images/Tpoint.png" alt="drawing" title="b" width="340"/>](/images/Tpoint.png)

<a name="Fig.2"></a>
Figure 2: Attribute Table of GPS Points.
<img src="/images/TP.jpg" alt="drawing"  width="680"/> 

<a name="Fig.3"></a>
#### Figure 3: Separated GPS Points Sets in Arc Catalog.
<img src="/images/Bike2.jpg" alt="drawing"  width="310"/>


<a name="Methodology"></a>
## Methodology
A [python script](https://github.com/simonscheider/mapmatching) is provided to allow [map-matching](https://en.wikipedia.org/wiki/Map_matching) (matching of track points to a network)  in arcpy using a Hidden Markov model with probabilities parameterized based on spatial and network distances. the following code block indicates the first part of mapMatch function python code:


``` Python
def mapMatch(track, segments, decayconstantNet = 25, decayConstantEu = 20, maxDist = 50, addfullpath = True):
```

 The main method is based on the [Viterbi algorithm for Hidden Markov models](https://en.wikipedia.org/wiki/Viterbi_algorithm). It gets trackpoints and segments, and returns the most probable segment path (a list of segments) for the list of points.
* Inputs:
* param track = a shape file (filename) representing a track, can also be unprojected (WGS84)
* param segments = a shape file of network segments, should be projected (in meter) to compute Euclidean distances properly (e.g. GCS Amersfoord)
* param decayconstantNet (optional) = the network distance (in meter) after which the match probability falls under 0.34 (exponential decay). (note this is the inverse of lambda). This depends on the point frequency of the track (how far are track points separated?)
* param decayConstantEu (optional) = the Euclidean distance (in meter) after which the match probability falls under 0.34 (exponential decay). (note this is the inverse of lambda). This depends on the positional error of the track points (how far can points deviate from their true position?)
* param maxDist (optional) = the Euclidean distance threshold (in meter) for taking into account segments candidates.
* param addfullpath (optional, True or False) = whether a contiguous full segment path should be outputted. If not, a 1-to-1 list of segments matching each track point is outputted.
note: depending on the type of movement, optional parameters need to be fine tuned to get optimal results.

the next plock code indicates the pythin code to match a route to each set of GPS point. Based on the following code, To use the above mapmatch function to match the path to each set of GPS points, firts all of the GPS point shape files are sorted based on their start time recorde or column "START_AT" to match the route according to the bike movement direction. in th enext step, through for loop, all GPS point sets are matched with a route which is a polyline with the same TRIP_ID of matched set of points. At the end with "arcpy.Merge_management()" all polyline merged in to gether to have an integrated dataset. In [Fig.4](), the green GPS points are matched with the grean route based the above python code. 

``` Python
    arcpy.env.workspace = your_path+'tPRROMTO\\AminSHP4'
    points3=arcpy.ListFeatureClasses()
    for Trksrt in points3:
        arcpy.Sort_management(Trksrt,your_path+'tPRROMTO\\AminShp2'+ '\\'+str(Trksrt),"end_at")
        # Process: Point_ID
        arcpy.AddField_management(your_path+'tPRROMTO\\AminShp2'+ '\\'+str(Trksrt), "Point_ID", "DOUBLE", "10", "10", "", "", "NULLABLE", "NON_REQUIRED", "")
        # Process: CAl_point_ID
        arcpy.CalculateField_management(your_path+'tPRROMTO\\AminShp2'+ '\\'+str(Trksrt), "Point_ID", "!FID!", "PYTHON_9.3", "")
        
arcpy.env.workspace = your_path+'tPRROMTO\\AminShp2'
roadname= your_path+'tPRROMTO\\AminShp\\Net_F.shp'
roadname2 =your_path+'tPRROMTO\\AminShp\\'
points=arcpy.ListFeatureClasses()
pp= arcpy.ListFeatureClasses("*","point","")
out_points=your_path+'tPRROMTO\\Amin_Improved\\'       

file_name_field = 'Trip_ID2'
for trackname in pp:
        arcpy.Statistics_analysis(trackname,out_points+"c_"+str( int(filter(str.isdigit, str(trackname)))), "FID count", "")
        cursor = arcpy.da.SearchCursor(out_points+"c_"+str( int(filter(str.isdigit, str(trackname)))), ['COUNT_FID'])
        for row in cursor:
             point_Size=str(row)
             #print int(filter(str.isdigit, str(point_Size)))  
          
             if  int(filter(str.isdigit, str(point_Size))) >2:
               arcpy.Copy_management(trackname, your_path+'tPRROMTO\\badFiles\\'+str( int(filter(str.isdigit, str(trackname))))+'.shp', "ShapeFile")  
               arcpy.Delete_management(trackname,"ShapeFile")
               arcpy.Delete_management(out_points+"c_"+str( int(filter(str.isdigit, str(trackname)))),"ArcInfoTable") 
             else:
               a=900
               b=50
               c=700       
               opt = mapMatch(trackname, roadname, a,b,c)
               exportPath(opt,trackname)
               arcpy.env.workspace = your_path+'tPRROMTO\\AminShp2'
               points2=arcpy.ListFeatureClasses()
               for trackname in points2:
                  #print(trackname) # just so you know what the script is processing
                  existing_fields = [f.name for f in arcpy.ListFields(trackname)]
                  if file_name_field not in existing_fields:
                    arcpy.management.AddField(trackname, file_name_field, 'text', field_length=200)
                  with arcpy.da.UpdateCursor(trackname, [file_name_field]) as uc:
                      for row in uc:
                          uc.updateRow([str(trackname)])
                  del row, uc
arcpy.env.workspace = your_path+'tPRROMTO\\AminShp24'
lines=arcpy.ListFeatureClasses()
arcpy.Merge_management(lines, "Merged_Paths.shp")
```
The another estimation in this project is estimating the shortest path between origin and destination of each bike rider or set of GPS points. Estimating the shortest of each set of GPS point is considerably similar to do the mapMatch the all points; the only different is that instead of considering all points in each set of GPS points (based on TRIP_ID), I considered the start point and the end point of each set. In other words, mapMatch function match a route between just two points, the start point (point with minimum of START_AT) and the end point (point with maximum of START_AT). The reason is that the Hidden Markov model try to find the shortest path between each pair of points, so the shortest path of each bike rider can be estimated by just considering the start and the end point of each GPS points set. The red polyline in [Fig.4](#Fig.4) indicates the shortest path between the start point and end the point of the green GPS points set.    

<a name="Fig.4"></a>
Figure 4: An Example of Matched Route and Shortest Path. 
[<img src="/images/Short_Long.png" alt="drawing" title="Fullscreen" width="500"/>](/images/Short_Long.png)
