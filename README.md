# GIS Workshop for ARCH 4509/6509

Tutorial written by Keith Jenkins, GIS Librarian at Mann Library, Cornell University, fall 2019.  
<https://kgjenkins.github.io/arch4509-fa2019/>

In this workshop, we will learn how use QGIS to:
* Import vector data (points, lines, polygons) from different file formats (shapefile, geopackage)
* Style data layers to visualize patterns in the attribute data
* Create subsets of data, based on location or attributes
* Export map images
* Export data to DXF format for use in software such as Rhino and AutoCAD


## Data sources

NYC Open Data provides public access to a wealth of data for New York City.  Similar websites exist for many (but not all!) other cities, states, and countries.

**Building Footprints**
  * polygon shapefile downloaded from [https://data.cityofnewyork.us/Housing-Development/Building-Footprints/nqwf-w8eh](https://data.cityofnewyork.us/Housing-Development/Building-Footprints/nqwf-w8eh)
  * metadata at [https://github.com/CityOfNewYork/nyc-geo-metadata/blob/master/Metadata/Metadata_BuildingFootprints.md](https://github.com/CityOfNewYork/nyc-geo-metadata/blob/master/Metadata/Metadata_BuildingFootprints.md)
  * clipped to Manhattan and saved as a GeoPackage (buildings.gpkg)

**Street Centerlines**
  * line shapefile downloaded from [https://data.cityofnewyork.us/City-Government/NYC-Street-Centerline-CSCL-/exjm-f27b](https://data.cityofnewyork.us/City-Government/NYC-Street-Centerline-CSCL-/exjm-f27b)
  * metadata in streets-metadata.pdf
  * removed ferry routes, selected Manhattan, exported to shapefile (streets.shp)

**Street Trees**
  * point shapefile downloaded from [https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35](https://data.cityofnewyork.us/Environment/2015-Street-Tree-Census-Tree-Data/pi5s-9p35)
  * metadata in trees-metadata.pdf
  * selected Manhattan, exported to geopackage (trees.gpkg)


## Step by Step

**1. Download the workshop data**

  * Download the workshop folder from [https://github.com/kgjenkins/arch4509-fa2019/archive/v1.zip](https://github.com/kgjenkins/arch4509-fa2019/archive/v1.zip)
  * Go to your downloads folder and unzip the file (right-click the file > 7-Zip > Extract Here)
  It is very important to unzip the the .zip file -- things will not work otherwise!


**2. Open QGIS**

* Start menu > QGIS 3

QGIS is a free, open-source geographic information system that can be used to create maps and perform spatial analysis.  If you would like to install QGIS on your own Windows, Mac, or Linux computer, visit the QGIS web site at [qgis.org](http://qgis.org/)


**3. Load the building footprints**

* Layer > Data Source Manager
* Select the "Vector" tab on the left
* Click the "..." button to browse to the `data` folder within the unzipped workshop files
* Select the `buildings.gpkg` file and click the "Open" button
* Click "Add" to add the selected file to your map
* Click "Close" to close the data source manager

"Vector" means points, lines, and polygons.  In this case, the building footprints are polygons.
Geopackage is a modern geospatial file format that can potentially contain multiple data layers, but in this example, it just contains a single layer called "buildings".  If it contained multiple layers, it would prompt you to select which layers you would like to add to your map.


**4. Basic Styling**

The buildings layer only contains data, and lacks any sense of style.  QGIS simply applies a default polygon style with a random color, but we can change it.

* Click the colorful paintbrush above the list of layers on the left in order to open the "Layer Styling" panel.
* Adjust the size and position of the panel as necessary.
* To change the color, click the color bar to choose a new color.  There are many ways to select a color!  (I prefer the triangle within the circle.)
* When you are done selecting a color, click the back arrow near the top of the the Layer Styling dock.
* Clicking the "Simple fill" part of the style will let you adjust other things, such as the stroke (border) color and width.


**5. Explore the building attribute data**

There are two basic ways to explore the data that is associated with each polygon:

* View the info for a specific polygon by selecting the "Identify Features" tool (blue circle with white "i") and then clicking a polygon.
* View the attribute table for the whole layer by right-clicking the layer name > Open Attribute Table

Notice that only some of the buildings have names in the data.  Other fields of interest include `CNSTRCT_YR` (year of construction), `HEIGHTROOF` (roof height above ground, in feet), and `GROUNDELEV` (ground elevation, in feet).  We can use the values of these fields to control the colors on the map.


**6. Graduated colors**

Whenever we have a field (or "column") that contains numbers, we can make our map use those values to set the color of the corresponding polygon.

* Open the "Layer Styling" dock and be sure that the tract boundary layer is selected.
* Change "Single symbol" (near the top) to "Graduated"
* Set Column = `CNSTRCT_YR`
* Click the "Classify" button near the bottom

The default classification mode is "Equal Interval", which doesn't work very well for `CNSTRCT_YR`, which apparently has values ranging from 0 (missing data) to 20000 (an error).

Change the mode to "Quantile" and observe the difference.  Also try "Natural Breaks", which is especially designed for mapping numeric data -- it tries to minimize the variation within each classification.  You can also double-click to edit the breakpoint values, for example, to set the bottom category to years 2000 and later.

You can change the number of classes (default is 5), and you can also change the color ramp -- click it to edit the colors.  There are many other ways to customize map styles in QGIS, such as blending modes and draw effects (in the Layer Rendering section) -- the possibilities are endless!


**7. Add a Basemap**

Sometimes it is useful to load a global base layer from the web, to add context to your map, or just to help confirmthat your data is correctly aligned.  The QuickMapServices plugin makes this easy.  (It's already installed on the Mann Library computers.)

* Web menu > QuickMapServices > CartoDB > Positron

If the labels on the basemap are pixelated (caused by the reprojection the basemap image), right-click the basemap layer > Set CRS > Set Project CRS from Layer.  (CRS means Coordinate Reference System.)


**8. Creating a Print Layout**

Now it's time to decide how our map will appear on a page, or in an exported image.  We can create a print layout that specifies the extent of the map we want to show (just Lower Manhattan, for example).  With a print layout, we can also add a title, a legend, a scale bar, and other elements to the page.

* Project menu > New Print Layout...
* Enter a name for the layout, such as "NYC buildings"

First, we add a map to the page:

* Add Item menu > Add Map
* Drag a box around the part of the page where you want the map.  (You can always adjust the sides later.)

By default, the map will be centered and scaled just as it appeared in the main QGIS window.  To adjust the extent of the map, use "Move Content" tool.

* Edit menu > Move Content

Now you can pan the map content, or zoom with the mouse wheel.  To zoom with finer control, hold the CTRL key while zooming.


**9. Add a title**

* Add Item > Add Label
* Drag a box where you want the title to appear.
* In the "Item Properties" on the right, change the default text "Lorem ipsum" to whatever you want for your title.
* Font options are found in the "Appearance" section below the text


**10. Add a Legend**

* Add Item > Add Legend
* Drag a box where you want the legend to appear.

The default legend is a bit ugly, as it uses the original layer names, and also shows layers that are not needed.  But we can customize the legend in the legend's Item Properties:

* Uncheck "Auto update" under Legend Items
* Select and delete (with the `-` button) any rows you don't want to show, like the basemap.
* Select `buildings` and click the "Edit" icon below (pencil on paper) to change the layer name to "Year of construction", for example.
* Fonts can be adjusted in the "Fonts" section of the Item Properties.


**11. Add a Scale Bar**

* Add Item > Add Scale Bar
* Drag a box where you want the scale bar to appear.
* Set scalebar units as desired, and experiment with the Segments settings


**12. Add a North Arrow**

Adding a north arrow is recommended whenever north is not towards the top of the page.

* Add Item > Add North Arrow
* Drag a box where you want the north arrow to appear.  Resize as desired.
* To change the color of the arrow, see the item properties.


**13. Export the map**

* Layout menu > Export as image (or PDF)

For quick map image exports (where you don't need a title, legend, north arrow, etc.) you can skip the process of creating a map layout:

* Project menu > Import/Export > Export Map to Image

Note that there is also an option to "Export Project to DXF", which is useful for getting GIS data into programs like Rhino or AutoCAD.


## Exploring lines and points

Try importing the Streets and Trees datasets.

**Streets** -- Note that the streets data is a shapefile, a common but archaic data format that consists of several different files, with extensions like .shp, .dbf, .shx, and more.  When adding a shapefile to QGIS, always select the .shp file.  The streets are lines, but can be styled in much the same way that we styled the polygons.

* Try styling the streets using a "categorized" style based on the `BIKE_LANE` column.  Streets with bike lanes will have a number 1-8, and can be highlighted with a color and thicker stroke, while leaving all other values as thin grey lines, for example.

**Trees** -- This is a geopackage of street trees inventorieda and maintained by the city.  When zoomed out, the points are too closely spaced to see them all.  Here's one method to better understand the densities across the map:

* In the Layer Styling panel, change the opacity to around 1-2%
* Combine that with reducing the point size

The trees data also has interesting information like overall tree health (`health`), or problems like sidewalk damage (`sidewalk`) or shoes hanging in the branches (`brnch_shoe`).  See trees-metadata.pdf for a full list and descriptions of the available attributes.


## Using data from OpenStreetMap

Many cities in the United States (like New York City) have open data websites where you can find all sorts of geospatial data for making maps.  However, that is not the case for many cities in other countries (or even some cities in the US) that either don't have the technological infrastructure for collecting and distributing such data, or lack policies promoting free and open access to data.

This data gap can often be filled by OpenStreetMap, a collaborative map of the world that anyone can edit.  OpenStreetMap (OSM) was founded in the UK in 2004, and now has mappers all over the world who trace satellite and aerial imagery to create the basic shapes of roads and buildings.  Details such as road names, business names, addresses, and much more are added by locals with more intimate knowledge of the places being mapped.  OSM is widely regarded as the most complete, accurate, and up-to-date map of the world, at least when it comes to roads.  Many cities (and countrysides) also have building outlines, and where those exist, the data is high quality.  But be aware that not all areas of the world (including some cities in the US) have not been completely mapped when it comes to buildings or more specialized features like businesses, drinking water sources, or fire hydrants.

To get a sense of what is on the map, explore the map at [openstreetmap.org](https://www.openstreetmap.org/)

Once you have zoomed in to a local area, you can use the "query features" tool to click the map to see the details for specific features.  OSM uses a tagging system to describe details.  For example, Mann Library currently includes these tags (among others):

```
amenity = library          building:levels = 4
building = yes             ele = 275
name = Mann Library        website = https://mannlib.cornell.edu/
```

The tagging system is [complex](https://wiki.openstreetmap.org/wiki/Map_Features#Building), but powerful, allowing 3rd-party tools to query the openly-licensed data to extract specific types of data.  For example, [this query in Overpass Turbo](https://overpass-turbo.eu/s/MgI) shows the results for drinking water sources in a very-well-mapped slum in Nairobi.

Any data you find in OSM can be downloaded and used in QGIS.  Here are some of the most popular sites for getting OSM data in GIS formats:

* [Overpass Turbo](https://overpass-turbo.eu) has a wizard that helps you build a search for specific words or tags, and thene export the results as a GeoJSON file that can be easily added to QGIS.  Overpass Turbo is best for relatively small queries that might only return 1000 results or less.

* The [HOT Export Tool](https://export.hotosm.org/) was created by the Humanitarian OpenStreetMap Team (HOT).  It lets you select an area of interest and a theme like "buildings", "education", or "healthcare" to download a geopackage or other format.  They have already done the hard work of determining what combinations of tags are used to describe features in the different themes.  The HOT Export Tool can export up to 10 million vertices at a time.  See their [Quick Start](https://export.hotosm.org/en/v3/learn/quick_start) tutorial for details.

* [Geofabrik](http://download.geofabrik.de/) offers shapefile extracts by country that are updated daily.

If there is time at the end of this workshop, try exploring the following data for Dhaka, Bangladesh that was extracted via the HOT Export Tool:

* <https://export.hotosm.org/downloads/ae820b92-fde7-4820-8421-079794917d29/dhaka-water_export_gpkg.zip>
* <https://export.hotosm.org/downloads/dde9da0b-9194-4979-b2ce-36cb2121a549/dhaka-transportation_export_gpkg.zip>
* <https://export.hotosm.org/downloads/b38ff5d9-ea03-483f-81fb-f2a3fe716c9e/dhaka-buildings_export_gpkg.zip>

Try using graduated colors for the buildings, but instead of selecting a column, type `$area` which is a built-in expression for the area of the polygon.  By applying a spectrum color ramp, it will suddenly becoming easier to spot the locations of slums that are made up of many tightly-packed small structures, for example.  Creative tinkering can lead to other interesting views of the data!

For a guide to other GIS Data Sources, see <https://github.com/kgjenkins/gis-data-sources>
