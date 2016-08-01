# FieldtypeLeafletMapMarker Module for ProcessWire

This is a port of the Map Marker Fieldtype by Ryan Cramer. Instead of Google maps it uses Leaflet maps.

Google maps geocoding is still used for geocoding default lat/lng values under *field* settings but the geocoding on page
editing uses Per Liedmans [leaflet-control-geocoder] (https://github.com/perliedman/leaflet-control-geocoder)

This Fieldtype for ProcessWire holds an address or location name, and automatically geocodes the address to latitude/longitude using leaflet-control-geocoder. The resulting values may be used to populate any kind of map (whether Leaflet Maps or another).

----------

## Installation

1. Copy all of the files for this module into /site/modules/FieldtypeLeafletMapMarker/

2. In your admin, go to the Modules screen and "check for new modules." Under the 'Fieldtype' section, install the 'Leaflet Map Marker' module (FieldtypeLeadletMapMarker)

3. Under the 'Inputfield' section, install the 'Leaflet Map Marker' module (InputfieldLeafletMapMarker) if it is not
   already installed.

4. Under the 'Markup' section, install the 'Leaflet Map' module (MarkupLeafletMap) **and** the 'Inline Scripts' module (MarkupAddInlineScript)

5. In your admin, go to Setup > Fields > Add New Field. Choose LeafletMapMarker as the type.
   If you are not sure what to name your field, simply "map" is a good one! Once created, configure the settings on the *input* tab.

6. Add your new "map" field to one or more templates, as you would any other field.


### How to use from the page editor

1. Create or edit a page using one of the templates you added the "map" field to.

2. Type in a location or address into the "address" box for the map field. Then click
   outside of the address, and the Javascript geocoder should automatically populate the
   latitude, longitude and map location. The Leaflet geocoder will accept full addresses
   or known location names. For instance, you could type in "Disney Land" and it knows
   how to find locations like that.

3. The geocoding also works in reverse. You may drag the map marker wherever you want
   and it will populate the address field for you. You may also populate the latitude,
   longitude and zoom fields manually if you like. Unchecking the box between address
   and latitude disables the geocoder.

_If the geocoding does not work, please ensure that your browser is not blocking the execution of any scripts._


### How to use from the API, in your template files

In your template files, you can utilize this data for your own Leaflet Maps (or anything else that you might need latitude/longitude for).

Lets assume that your field is called 'map'. Here is how you would access the components of it from the API:
```````````
echo $page->map->address;	// outputs the address you entered
echo $page->map->lat; 		// outputs the latitude
echo $page->map->lng; 		// outputs the longitude
echo $page->map->zoom;		// outputs the zoom level
`````````

-------------

## Markup Leaflet Map

This package also comes with a markup helper module called MarkupLeafletMap. It provides a simple means of outputting a Leaflet Map based on the data managed by FieldtypeLeafletMapMarker.

### Basic Usage

Seeing as we are going to need access to the module as we generate the HTML header block (to add script and style
includes), we first load and gain access to the Markup module...
```
<?php $map = wire('modules')->get('MarkupLeafletMap'); ?>
```

_Leaflet's js code requires jquery - so make sure you load that in HTML header section, the markup module will not do this for you._

MarkupLeafletMap includes the _getLeafletMapHeaderLines()_ method that generates all the needed script and style includes for your HTML header
section so simply add this somewhere before your closing `</head>` tag:
```
<?php echo $map->getLeafletMapHeaderLines(); ?>
```

In the location within the body of your HTML where you want your map to appear, place the following:
`````````
<?php echo $map->render($page, 'YOUR MARKER FIELD'); ?>
`````````

To render a map with multiple markers on it, specify a PageArray rather than a single $page:
`````````
$items = $pages->find("A SELECTOR THAT GETS YOUR PAGES WITH MARKER FIELDS");
echo $map->render($items, 'YOUR MARKER FIELD');
`````````

To specify options, provide a 3rd argument with an options array:
`````````
echo $map->render($items, 'YOUR MARKER FIELD', array('height' => '500px'));
`````````

### Options

Consult the following table for more options for customising your Leaflet map.

Option | Notes
--------------
`width` | Width of the map (type: string; default: 100%).
`height` | Height of the map (type: string; default: 300px)
`zoom` | Zoom level 1-25 (type: integer; default: from your field settings)
`id` | Map ID attribute (type: string; default: mgmap)
`class` | Map class attribute (type: string; default: MarkupLeafletMap)
`lat` | Map center latitude (type: string|float; default: from your field settings)
`lng` | Map center longitude (type: string|float; default: from your field settings)
`useStyles` | Whether to populate inline styles to the map div for width/height (type: boolean; default: true). Set to false only if you will style the map div yourself.
`useMarkerSettings` | Makes single-marker map use marker settings rather than map settings (type: boolean; default: true).
`markerLinkField` | Page field to use for the marker link, or blank to not link (type: string; default: url).
`markerTitleField` | Page field to use for the marker title, or blank not to use a marker title (type: string; default: title).
`fitToMarkers` | When multiple markers are present, set map to automatically adjust to fit to the given markers (type: boolean; default: true).

----------

## Customising A Marker's Visuals and Popup Contents

As part of the options array, you can specify two callback functions. The first can customise the visual look of the marker -
including its colour and icon. The second allows you to add additional content to the popup that appears when a map
marker is clicked.


---------

### Contributors

* [Ryan Cramer](https://processwire.com/talk/profile/2-ryan/) provided the original Google Maps module.
* [Mats](https://processwire.com/talk/profile/67-mats/) produces the original Leaflet version.
* [gebeer](https://processwire.com/talk/profile/1920-gebeer/) extended Mats' work, adding the Inline Scripts module.
* [netcarver](https://processwire.com/talk/profile/465-netcarver/) added callback formatters for marker and popover content generation. He also added AwesomeMarker
  support.
