
# Getting Winkel Tripel map projection enabled in GDAL 1.10.1 (QGIS, GRASS GIS etc.)

Winkel Tripel map projection is still blacklisted in GDAL. Since it is one of favorite map projections for world maps, with this custom install of GDAL one can use this map projection in QGIS or GRASS GIS just as any other already supported map projection.

These instructions are for Debian Linux system. Adapt it to your Linux system if necessary.

## Proj4

First check if proj is installed and which version it is 
```
proj
```
If not installed then you get:
```
bash: proj: command not found
```
If installed you get version:
```
Rel. 4.9.3, 15 August 2016
usage: proj [ -bCeEfiIlormsStTvVwW [args] ] [ +opts[=arg] ] [ files ]
```
If your version is 4.9.1 or higher then you have version with inverse Winkel Tripel (and Aitoff) solution. Continue with GDAL compilation.

If your version is older then compile proj4 from source. First download ZIP from https://github.com/OSGeo/proj.4. You will get proj.4-master.zip file. Make sure you have development packages installed. In folder with zip file execute:
```
unzip proj.4-master.zip
cd proj.4-master
./configure
make
```
As a root execute:
```
make install
```
Check it again:
```
proj
Rel. 4.9.3, 15 August 2016
usage: proj [ -bCeEfiIlormsStTvVwW [args] ] [ +opts[=arg] ] [ files ]

proj +proj=wintri +ellps=sphere +lat_1=50.459776252 -I
10000 10000
0d6'35.64"E	0d5'23.756"N
```
## GDAL 1.10.1+wintri

As a root execute:
```
apt-get build-dep gdal
```
Download gdal-1.10.1+dfsg_with_winkel_tripel.tar.gz from https://github.com/GEOF-OSGL/gdal-1.10.1-wintri

In folder with zip file execute:
```
tar -xvzf gdal-1.10.1+dfsg_with_winkel_tripel.tar.gz
cd gdal-1.10.1+dfsg
./configure
make
```
As a root execute:

```
make install
```

## QGIS

As a root execute:
```
apt-get install qgis
cd /usr/lib
rm libgdal*
ln -s /usr/local/lib/libgdal.so.1.17.1 libgdal.so.1
rm libproj*
ln -s /usr/local/lib/libproj.so.12.0.0 libproj.so
ln -s /usr/local/lib/libproj.so.12.0.0 libproj.so.0
ldconfig
```
Finally, you may to set environment variable:
```
export GDAL_DATA=/usr/local/share/gdal
```
And start QGIS
```
qgis
```

Your QGIS (and GRASS GIS) should now support Winkel Tripel map projection (if it was compiled against GDAL 1).

QGIS have some visualisation issues when some projections are used on the fly. Save or warp layer in wintri CRS to overcome these issues.
