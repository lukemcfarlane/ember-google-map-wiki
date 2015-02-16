### Introduction

Any `lat` or `lng` properties in the following documentation refer respectively to the latitude and longitude values of the concerned object. They are of type `number`.


## Main `{{google-map}}` component

THe main thing of this addon is of course the map component, which you can use in your templates with `{{google-map}}`.

Everything then defined as properties (and described below) are then bound with 2 way data-binding, which means that everything done within your Ember application code will be reflected on the map, and everything done on the map will be reflected in your Ember objects. This also means that you should not use the same object or properties to, for example, define the center of the map as the first marker position. If you want to do this, you'll need to have another property defined in your controller linked to that first marker position using `Ember.computed.reads` (or `oneWay`).

### Basic properties

- **`lat`** and **`lng`** (`number`, default: `0`)

    Those properties are use to define (or track) the center of the map.

- **`type`** (`string`, default: `road`)

    The type of the map. Its value can be `road`, `satellite`, `terrain` or `hybrid`.

- **`zoom`** (`number`, default: `5`)

    The zoom level of the map. It's a `number` between `0` and `18` but it has limitation depending the type of map you're using. See [Google documentation](https://developers.google.com/maps/documentation/javascript/maxzoom).

- **`fitBoundsArray`** (`Array.<{lat: number, lng: number}>`, default: _automatic depending on `autoFitBounds`_)

    An array of objects containing `lat` and `lng` coordinates. All coordinates will be used to center and zoom the map so that it can render all of the given positions. Do not set this if you defined `autoFitBounds` to something else than `false` (see below).

- **`autoFitBounds`** (`boolean` or `string`, default: `false`)

    This can be a boolean, in which case if `false` it'll do nothing, else if `true` it'll automatically center and zoom the map so that all [[objects you defined on the map|Provided-Tools-and-Classes-(API)#defining-objects-to-draw]] would be visible on the map.

    It can also be a `string` containing the list of type of objects which you want to all fit in the map. It's like if you set it to `true`, except it'll take care only of the objects of given type(s). For example `markers,circles` would center and zoom the map so that all markers and circle will be visible, but won't take care of other objects on the map. Valid types are: `markers`, `infoWindows`, `circles`, `polylines` and `polygons`.


### Google options as properties

Any [Google Maps option](https://developers.google.com/maps/documentation/javascript/reference#MapOptions) which is not handled by a component property can be set using the same option name prefixed with **`gopt_`**.

For example to disable the zoom controls (Google Maps option named `zoomControl`):

```handlebars
{{google-map ... gopt_zoomControl=false}}
```

This is valid for any [[view|Provided-Tools-and-Classes-(API)#views]] corresponding to a Google Maps object. Refer to the [Google Maps documentation](https://developers.google.com/maps/documentation/javascript/reference) for possible settings.


### Defining objects to draw

Any drawable object (also called _overlay_ by Google) of the map has its corresponding **array** property on the component. Setting those properties with your own arrays let you define what have to be drawn on the map. Here are the possible things you can draw on the map (and track updates):

#### Markers
<img align="right" src="assets/marker.png" height="100">
Google type: [google.maps.Marker](https://developers.google.com/maps/documentation/javascript/reference#Marker)
- Component property: **`markers`**
- Events: **`click`**, **`dblclick`**, **`drag`**, **`dragend`**, **`dragstart`**, **`mousedown`**, **`mouseout`**, **`mouseover`**, **`mouseup`** and **`rightclick`**
- Special component properties:
    - **`markerController`**: name of the controller to use for each marker, must extend `google-map/marker` controller
    - **`markerViewClass`**: name or class of the view to use for each marker, must extend `google-map/marker` view
    - **`markerInfoWindowTemplateName`**: name of the template to use for all markers' info-window
    - **`markerHasInfoWindow`**: whether all marker with undefined `hasInfoWindow` should have an info-window or not

**Each marker has these properties:**

- **`lat`** and **`lng`**: coordinates of the marker, **mandatory**
- **`isClickable`** (`boolean`): whether the marker is clickable or not
- **`isVisible`** (`boolean`): whether the marker is visible or not
- **`isDraggable`** (`boolean`): whether the marker is draggable or not
- **`title`** (`string`): title of the marker (visible on hover)
- **`opacity`** (`number`): opacity of the marker
- **`icon`** (`string` or [`google.maps.Icon`](https://developers.google.com/maps/documentation/javascript/reference#Icon)): icon of the marker
- **`zIndex`** (`number`): z-index of the marker
- **`hasInfoWindow`** (`boolean`): whether there is an info-window attached to this marker or not
- **`description`** (`string`): will be used to fill the attached info-window if no template have been specified
- **`isInfoWindowVisible`** (`boolean`): whether the attached info-window is visible or not
- **`infoWindowTemplateName`** (`string`): template to be used with the info-window

#### Info Windows
<img align="right" src="assets/info-window.png" height="100">
- Google type: [google.maps.InfoWindow](https://developers.google.com/maps/documentation/javascript/reference#InfoWindow)
- Component property: **`infoWindows`**
- Events: **`closeclick`** and **`domready`**
- _If you want to attach an info-window to a marker, prefer using the built-in marker's info-window related properties (see above)_
- Special component properties:
    - **`infoWindowController`**: name of the controller to use for each info-window, must extend `google-map/info-window` controller
    - **`infoWindowViewClass`**: name or class of the view to use for each info-window, must extend `google-map/info-window` view
    - **`infoWindowTemplateName`**: name of the template to use for each info-window

**Each info-window has these properties:**
- **`lat`** and **`lng`**: coordinates of the info-window, **mandatory**
- **`isVisible`** (`boolean`): whether the info-window is visible or not
- **`zIndex`** (`number`): z-index of the info-window
- **`title`** (`string`): title of the info-window
- **`description`** (`string`): body of the info-window
- **`templateName`** (`string`): name of the template to use with the info-window

#### Circles
<img align="right" src="assets/circle.png" height="100">
- Google type: [google.maps.Circle](https://developers.google.com/maps/documentation/javascript/reference#Circle)
- Component property: **`circles`**
- Events: **`click`**, **`dblclick`**, **`drag`**, **`dragend`**, **`dragstart`**, **`mousedown`**, **`mouseout`**, **`mouseover`**, **`mouseup`** and **`rightclick`**
- Special component properties:
    - **`circleController`**: name of the controller to use for each circle, must extend `google-map/circle` controller
    - **`circleViewClass`**: name or class of the view to use for each circle, must extend `google-map/circle` view

**Each circle has these properties:**
- **`lat`** and **`lng`**: coordinates of the circle, **mandatory**
- **`radius`** (`number`): radius of the circle (in meters), **mandatory**
- **`isClickable`** (`boolean`): whether the circle is clickable or not
- **`isVisible`** (`boolean`): whether the circle is visible or not
- **`isDraggable`** (`boolean`): whether the circle is draggable or not
- **`isEditable`** (`boolean`): whether the circle is editable or not
- **`strokeColor`** (`string`): css color of the circle's stroke (read-only after creation)
- **`strokeOpacity`** (`number`): opacity of the circle's stroke (read-only after creation)
- **`strokeWeight`** (`number`): weight of the circle's stroke (read-only after creation)
- **`fillColor`** (`string`): css color of the circle's fill (read-only after creation)
- **`fillOpacity`** (`number`): opacity of the circle's fill (read-only after creation)
- **`zIndex`** (`number`): z-index of the circle

#### Polyline
<img align="right" src="assets/polyline.png" height="100">
- Google type: [google.maps.Polyline](https://developers.google.com/maps/documentation/javascript/reference#Polyline)
- Component property: **`polylines`**
- Events: **`click`**, **`dblclick`**, **`drag`**, **`dragend`**, **`dragstart`**, **`mousedown`**, **`mouseout`**, **`mouseover`**, **`mouseup`** and **`rightclick`**
- Special component properties:
    - **`polylineController`**: name of the controller to use for each polyline, must extend `google-map/polyline` controller
    - **`polylinePathController`**: name of the controller to use for each polyline path, must extend `google-map/polyline-path` controller
    - **`polylineViewClass`**: name or class of the view to use for each polyline, must extend `google-map/polyline` view

**Each polyline has these properties:**
- **`lat`** and **`lng`**: coordinates of the polyline, **mandatory**
- **`path`** (`Array.<{lat: number, lng: number}>`): path of the polyline (each item represent a point of the path), **mandatory**
- **`icons`** ([`Array.<google.maps.IconSequence>`](https://developers.google.com/maps/documentation/javascript/reference#IconSequence)): the icons to be rendered along the polyline
- **`isClickable`** (`boolean`): whether the polyline is clickable or not
- **`isVisible`** (`boolean`): whether the polyline is visible or not
- **`isDraggable`** (`boolean`): whether the polyline is draggable or not
- **`isEditable`** (`boolean`): whether the polyline is editable or not
- **`isGeodesic`** (`boolean`): whether the polyline is geodesic or not (when `true` the edges follow the curvature of the Earth)
- **`strokeColor`** (`string`): css color of the polyline's stroke (read-only after creation)
- **`strokeOpacity`** (`number`): opacity of the polyline's stroke (read-only after creation)
- **`strokeWeight`** (`number`): weight of the polyline's stroke (read-only after creation)
- **`zIndex`** (`number`): z-index of the polyline

#### Polygon
<img align="right" src="assets/polygon.png" height="100">
- Google type: [google.maps.Polygon](https://developers.google.com/maps/documentation/javascript/reference#Polygon)
- Component property: **`polygons`**
- Events: **`click`**, **`dblclick`**, **`drag`**, **`dragend`**, **`dragstart`**, **`mousedown`**, **`mouseout`**, **`mouseover`**, **`mouseup`** and **`rightclick`**
- Special component properties:
    - **`polygonController`**: name of the controller to use for each polygon, must extend `google-map/polygon` controller
    - **`polygonPathController`**: name of the controller to use for each polygon path, must extend `google-map/polygon-path` controller
    - **`polygonViewClass`**: name or class of the view to use for each polygon, must extend `google-map/polygon` view

**Each polygon has these properties:**
- **`lat`** and **`lng`**: coordinates of the polygon, **mandatory**
- **`path`** (`Array.<{lat: number, lng: number}>`): path of the polygon (each item represent a point of the path), **mandatory**
- **`icons`** ([`Array.<google.maps.IconSequence>`](https://developers.google.com/maps/documentation/javascript/reference#IconSequence)): the icons to be rendered along the polygon
- **`isClickable`** (`boolean`): whether the polygon is clickable or not
- **`isVisible`** (`boolean`): whether the polygon is visible or not
- **`isDraggable`** (`boolean`): whether the polygon is draggable or not
- **`isEditable`** (`boolean`): whether the polygon is editable or not
- **`isGeodesic`** (`boolean`): whether the polygon is geodesic or not (when `true` the edges follow the curvature of the Earth)
- **`strokeColor`** (`string`): css color of the polygon's stroke (read-only after creation)
- **`strokeOpacity`** (`number`): opacity of the polygon's stroke (read-only after creation)
- **`strokeWeight`** (`number`): weight of the polygon's stroke (read-only after creation)
- **`fillColor`** (`string`): css color of the polygon's fill (read-only after creation)
- **`fillOpacity`** (`number`): opacity of the polygon's fill (read-only after creation)
- **`zIndex`** (`number`): z-index of the polygon


## Controllers

### Array controllers
### Associated object controllers (item controllers)
### Customising the path to `lat`, `lng`, ...


## Views

### All views
### Creating an `InfoWindow` template
### Listening to Google events
### Accessing the Google map object (not recommended)
