# SS_SYVETA
/*Identifying Encroachments Happening On Streets /Water Bodies In & Around Madurai*/
/*importing the necessary header files*/
import pandas as pd
import os 
import fileinput
import csv
import random
import pyproj
import shapely
import shapely.ops as ops

/*finding the directory of csv file (dataset)*/
data = pd.read_csv("SS.csv",encoding='utf-8')
print(type(data))

out:
<class 'pandas.core.frame.DataFrame'>

/*directory path of dataset*/
print("the directory we are working in is {}".format(os.getcwd()))
data = pd.read_csv("SS.csv")
print(type(data))

out:
the directory we are working in is C:\Users\HP Windows 10\Desktop
<class 'pandas.core.frame.DataFrame'>


data.shape

Out[20]:(58, 4)


data.ndim

Out[21]:2

/*fetching data from dataset*/
SS = pd.read_csv("SS.csv")

SS['id'] = [random.randint(0,1000) for x in range(SS.shape[0])]

SS.head(8)

	Place_Name		Latitudes	Longitudes	Area		Id
0	Teppakulam_SE		9.911742	780146699	0.00000	378
1	Teppakulam_SC		9.911634	78.148057	0.00000	526
2	Teppakulam_SW	9.911543	78.149391	0.00000	402
3	Teppakulam_WC	9.910271	78.149313	0.00000	842
4	Teppakulam_NW	9.909020	78.149227	0.00000	813
5	Teppakulam_NC	9.909124	78.147875	0.00000	719
6	Teppakulam_NE	9.909217	78.146490	0.00000	789
7	Teppakulam_EC		9.910368	78.146574	17638.56053	60


 







/*table format of dataset*/

Out:
 


/*finding the area of government place ,here we took Teppakulam area as sample data*/
from shapely.geometry.polygon import Polygon

from functools import partial

geom1 = Polygon([(9.91174209511721,78.14669941153626),(9.911633639510441,78.14805712007002),(9.911543051294258,78.1493905611933),(9.910271029688554,78.14931290490678),(9.909019637730102,78.14922656336422),(9.909124318945034,78.14787524095665),(9.90921692189016,78.146489688379),(9.910367909573674,78.14657398634154)])

geom_area = ops.transform(
    partial(
        pyproj.transform,
        pyproj.Proj(init='EPSG:4326'),
        pyproj.Proj(
            proj='aea',
            lat1=geom1.bounds[1],
            lat2=geom1.bounds[3])),
geom1)
print(geom_area.area)

out:
17638.560630825203

/*finding whether it is the encroached area or not*/
from shapely.geometry import Point

from shapely.geometry.polygon import Polygon

point = Point(9.91174209511721, 78.14669941153626)
polygon = Polygon([(9.91174209511721, 78.14669941153626), (9.911633639510441, 78.14805712007002), (9.911543051294258, 78.1493905611933), (9.910271029688554, 78.14931290490678), (9.909019637730102, 78.14922656336422), (9.909124318945034, 78.14787524095665), (9.90921692189016, 78.146489688379), (9.910367909573674, 78.14657398634154)])
polygon.contains(point)

false

polygon.area

out:
6.887812400802526e-06

/*a point for Encroached area*/
point1 = Point()
polygon1 = Polygon([(9.927192412969998,78.1322240162612),(9.928770590233473,78.12895366114391),(9.926355003328483,78.12797273945763),(9.92540451977902,78.13130787450065)])
if polygon1.contains(point1):
    print("Encroached Area")
else:
    print("UnEncroached Area")

out:
Encroached Area

/* a point for unencroached area*/
point1 = Point(9.927192412969998,78.1322240162612)
polygon1 = Polygon([(9.927192412969998,78.1322240162612),(9.928770590233473,78.12895366114391),(9.926355003328483,78.12797273945763),(9.92540451977902,78.13130787450065)])
if polygon1.contains(point1):
    print("Encroached Area")
else:
    print("UnEncroached Area")

out:
UnEncroached Area

/*Solution: While paying EB bills each and every time the Public is requested to submit their Property Coordinates at the time of Payment so by this the Encroached area is detected and informed to higher authorities to take actions towards it. As soon as the area is found as Encroached during the EB bill Payment the EB connections are suddenly cutdownned to those who do these illegal practices.*/
