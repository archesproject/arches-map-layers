## Examples of Additional Map Layers for Arches 4

This repository is intended to hold working examples of the different types of geospatial data formats that can be added to Arches 4 as map overlays. Feel free to clone this repo locally, or make a pull request to add another example.

Note that where `-b` occurs in the commands below it is used designate the new layer as a basemap. By default, new layers are considered overlays. Any of the examples in this repository could be either basemaps or overlays.

You can clone this repo and then copy/paste the example code below, making sure to change file paths appropriately.

Note that the mapnik renderer (which can be used with tileserver layers) is not supported on Windows.

### Adding MapBox Style Layers

##### MapBox basemap style

A MapBox style designed in [MapBox Studio](https://www.mapbox.com/studio/). When you create a new style (or modify an existing one), just publish it, download the style specification, and use something like the command below.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/Outdoor/style.json" -n "Outdoor" -b`

[style.json](https://github.com/legiongis/arches4-geo-examples/blob/master/mapbox_layers/Outdoors/style.json)

##### Another MapBox example

A 3d rendering of buildings data. Also made in MapBox studio.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/3d_Buildings/3d-buildings-mapbox.json" -n "3d Buildings"`

[3d-buildings-mapbox.json](https://github.com/legiongis/arches4-geo-examples/blob/master/mapbox_layers/3d_Buildings/3d-buildings-mapbox.json)

### Tiles From Non-Mapbox Sources

You can add tile layers from sources other than Mapbox, both static tiles (XYZ) or vector tiles. If you use vector tiles from a non-Mapbox source, you will most likely need to update your Mapbox Glyphs in Arches System Settings. This is a global setting that Arches uses to look for fonts that are used in vector tile layers.

Tiles must be published in EPSG:3857 (web mercator).

##### XYZ tileset

This example from [Stamen](http://maps.stamen.com/#watercolor/12/37.7706/-122.3782).

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/XYZ/stamen-watercolor.json" -n "Watercolor" -b`

[stamen-watercolor.json](https://github.com/legiongis/arches4-geo-examples/blob/master/mapbox_layers/XYZ/stamen-watercolor.json)

##### maptiler.com Vector Tiles

A free account at Maptiler.com gives you access to 100,000 requests/month. Create an account, go to Maps, choose one, and followed the link to json style spec, something like https://maps.tilehosting.com/styles/topo/style.json?key=xn30z7c3rAoDBq6lXGRk.

Download this spec to a .json file, and load it with `add_mapbox_layer`, or you can use the example in this repo:

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/MapTiler/maptiler-topo-vectortiles.json" -n "Topo"`

[maptiler-topo-vectortiles.json](https://github.com/legiongis/arches4-geo-examples/blob/master/mapbox_layers/MapTiler/maptiler-topo-vectortiles.json)

Finally, in Arches System Settings (in the app UI) update the Map Settings > Mapbox Glyphs setting with the "glyphs" url that can be found in the .json spec that you downloaded.

##### ArcGIS Server Vector Tiles

*We are currently looking for an example of a vector tile service on an ArcGIS server which a) uses open data and b) published tile in the correct spatial reference (EPSG:3857).*

### Adding Tileserver Layers

##### vector tiles from PostGIS table

First load the `rivers.sql` file into your PostGIS database.

`psql -U postgres -h localhost -d <your Arches db name> -f arches4-geo-examples/tileserver/postgis/rivers.sql`

Creates a new table called `rivers` in the `arches` database and loads it with the included file. Then use

`python manage.py packages -o add_tileserver_layer -t "arches4-geo-examples/tileserver/postgis/rivers.json" -n "rivers"`

[rivers.json](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/postgis/rivers.json)

##### from shapefile

This is an example of using the mapnik rendering engine in a tileserver layer. Note the `-m` argument used to point to a mapnik .xml file.

`python manage.py packages -o add_tileserver_layer -m "arches4-geo-examples/tileserver/shapefile/world.xml" -n "world"`

[world.xml](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/shapefile/world.xml)

##### from geotiff

Another mapnik example, in this case serving a geotiff (in EPSG:3857).

`python manage.py packages -o add_tileserver_layer -m "arches4-geo-examples/tileserver/geotiff/hillshade.xml" -n "hillshade"`

[hillshade.xml](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/geotiff/hillshade.xml)

##### using the tileserver to cascade a WMS

You can also create a new tileserver layer to add an external Web Map Service to your Arches map. This repo contains two examples

1. From ArcGIS Server

    ###### precipitation layer from US NOAA

    `python manage.py packages -o add_tileserver_layer -t "arches4-geo-examples/tileserver/wms/wms-noaa.json" -n "NOAA Precipitation"`

    [wms-noaa.json](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/wms/wms-noaa.json)
    
    Note that in the url for this example, the `layers` parameter in the url has a value of `5`. You'll need to update this to the correct layer number in your ArcGIS server. Often this is `0`.
    
    ###### nautical charts from [US NOAA](https://nauticalcharts.noaa.gov/data/gis-data-and-services.html#seamless-raster-nautical-chart-services)

    `python manage.py packages -o add_tileserver_layer -t "arches4-geo-examples/tileserver/wms/wms-nautical-charts.json" -n "US Nautical Charts"`

    [wms-nautical-charts.json](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/wms/wms-nautical-charts.json)

2. From GeoServer

    ###### a historic map mosaic centered around Natchitoches, Louisiana, USA (31.751892, -93.085166)
   
    `python manage.py packages -o add_tileserver_layer -t "arches4-geo-examples/tileserver/wms/wms-geoserver.json" -n "Confederate Maps"`

    [wms-geoserver.json](https://github.com/legiongis/arches4-geo-examples/blob/master/tileserver/wms/wms-geoserver.json)

    Note that when retrieving a WMS from GeoServer, the `layer` parameter must contain the layer name prepended with the workspace: `crnha:confedmaps_full` is the `confedmaps_full` layer in the `crnha` workspace.
