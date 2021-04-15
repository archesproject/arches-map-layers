## Examples of Additional Map Layers for Arches 5

This repository is intended to hold working examples of the different types of geospatial data formats that can be added to Arches 5 as map overlays. Feel free to clone this repo locally, or make a pull request to add another example.

Note that where `-b` occurs in the commands below it is used designate the new layer as a basemap. By default, new layers are considered overlays. Any of the examples in this repository could be either basemaps or overlays.

You can clone this repo and then copy/paste the example code below, making sure to change file paths appropriately.

### Adding MapBox Style Layers

##### MapBox basemap style

A MapBox style designed in [MapBox Studio](https://www.mapbox.com/studio/). When you create a new style (or modify an existing one), just publish it, download the style specification, and use something like the command below.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/Outdoor/style.json" -n "Outdoor" -b`

[style.json](https://github.com/archesproject/arches-map-layers/blob/master/mapbox_layers/Outdoors/style.json)

##### Another MapBox example

A 3d rendering of buildings data. Also made in MapBox studio.

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/3d_Buildings/3d-buildings-mapbox.json" -n "3d Buildings"`

[3d-buildings-mapbox.json](https://github.com/archesproject/arches-map-layers/blob/master/mapbox_layers/3d_Buildings/3d-buildings-mapbox.json)

### Tiles From Non-Mapbox Sources

You can add tile layers from sources other than Mapbox, both static tiles (XYZ) or vector tiles. If you use vector tiles from a non-Mapbox source, you will most likely need to update your Mapbox Glyphs in Arches System Settings. This is a global setting that Arches uses to look for fonts that are used in vector tile layers.

Tiles must be published in EPSG:3857 (web mercator).

##### XYZ tileset

This example from [Stamen](http://maps.stamen.com/#watercolor/12/37.7706/-122.3782).

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/XYZ/stamen-watercolor.json" -n "Watercolor" -b`

[stamen-watercolor.json](https://github.com/archesproject/arches-map-layers/blob/master/mapbox_layers/XYZ/stamen-watercolor.json)

##### maptiler.com Vector Tiles

A free account at Maptiler.com gives you access to 100,000 requests/month. Create an account, go to Maps, choose one, and followed the link to json style spec, something like https://maps.tilehosting.com/styles/topo/style.json?key=xn30z7c3rAoDBq6lXGRk.

Download this spec to a .json file, and load it with `add_mapbox_layer`, or you can use the example in this repo:

`python manage.py packages -o add_mapbox_layer -j "arches4-geo-examples/mapbox_layers/MapTiler/maptiler-topo-vectortiles.json" -n "Topo"`

[maptiler-topo-vectortiles.json](https://github.com/archesproject/arches-map-layers/blob/master/mapbox_layers/MapTiler/maptiler-topo-vectortiles.json)

Finally, in Arches System Settings (in the app UI) update the Map Settings > Mapbox Glyphs setting with the "glyphs" url that can be found in the .json spec that you downloaded.

##### ArcGIS Server Vector Tiles

*We are currently looking for an example of a vector tile service on an ArcGIS server which a) uses open data and b) published tile in the correct spatial reference (EPSG:3857).*

### Configuring a Web Map Service

A Web Map Service (WMS) can be configured as a layer source in Arches. The configuration is composed of a Map Source
and a Map Layer and can be configured in the Django admin pages found at `http://<Arches host>:<Arches port>/admin`.
A Map Source / Map Layer can also be loaded in to the system by way of a Django Fixture which needs to be run explicitly
or a Django Migration which is run automatically.

##### Django Fixtures
Map Sources and Layers are loaded as a Django Fixture by creating a file in either JSON or YAML format in the
`<Arches Project Directory>/fixtures` directory and loaded by running

`# python manage.py loaddata <fixture name>` eg

`# python manage.py loaddata bc-regional-district-boundaries-vector`

See [bc-regional-district-boundaries-vector.json](https://github.com/archesproject/arches-map-layers/blob/master/wms/fixtures/bc-regional-district-boundaries-vector.json)

where the fixture name is the filename of the fixture without the file extension. Django Fixtures are loaded manually.
If the data should be automatically loaded it can be done as a Django Extension

##### Django Migration

To be added.
