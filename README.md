## Examples of Additional Map Layers for Arches 4

This repository is intended to hold working examples of the different types of geospatial data formats that can be added to Arches 4 as map overlays. Feel free to clone this repo locally, or make a pull request to add another example.

Note that where `-b` occurs in the commands below it is used designate the new layer as a basemap. By default, new layers are considered overlays. Technically speaking, any of examples in this repository could be either basemaps or overlays.

You can clone this repo and then copy/paste the example code below, making sure to change file paths appropriately.

Note that the mapnik renderer (which can be used with tileserver layers) is not supported on Windows.

### Adding MapBox Style Layers

##### MapBox basemap style

A MapBox style designed in [MapBox Studio](https://www.mapbox.com/studio/). When you create a new style (or modify an existing one), just publish it, download the style specification, and use something like the command below.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/Outdoor/style.json" -n "Outdoor"`

##### Another MapBox example

A 3d rendering of buildings data. Also made in MapBox studio.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/3d_Buildings/3d-buildings-mapbox.json" -n "3d Buildings"`

### Adding Tileserver Layers

###### vector tiles from PostGIS table

First load the `rivers.sql` file into your PostGIS database.

`psql -U postgres -h localhost -d arches -f arches4-geo-examples/tileserver/postgis/rivers.sql`

Creates a new table called `rivers` in the `arches` database and loads it with the included file. Then use

`python manage.py packages -o add_tileserver_layer -t "arches4-geo-examples/tileserver/postgis/rivers.json" -n "rivers"`

###### from shapefile

This is an example of using the mapnik rendering engine in a tileserver layer. Note the `-m` argument used to point to a mapnik .xml file.

`python manage.py packages -o add_tileserver_layer -m "arches4-geo-examples/tileserver/shapefile/world.json" -n "world"`

###### from geotiff

Another mapnik example, in this case serving a geotiff (in EPSG:3857).

`python manage.py packages -o add_tileserver_layer -m "arches4-geo-examples/tileserver/geotiff/hillshade.json" -n "hillshade"`
