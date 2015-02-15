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

Any drawable object (also called _overlays_ by Google) of the map has its corresponding array property on the component. Here are the possible things you can draw on the map (and track updates):


#### Markers
<img align="right" src="assets/marker.png" height="80">
- Google Maps: [google.maps.Marker](https://developers.google.com/maps/documentation/javascript/reference#Marker)
- Component property: **`markers`**

#### Info Windows
<img align="right" src="assets/info-window.png" height="80">
- Google Maps: [google.maps.InfoWindow](https://developers.google.com/maps/documentation/javascript/reference#InfoWindow)
- Component property: **`infoWindows`**

#### Circles
<img align="right" src="assets/circle.png" height="80">
- Google Maps: [google.maps.Circle](https://developers.google.com/maps/documentation/javascript/reference#Circle)
- Component property: **`circles`**

#### Polyline
<img align="right" src="assets/polyline.png" height="80">
- Google Maps: [google.maps.Polyline](https://developers.google.com/maps/documentation/javascript/reference#Polyline)
- Component property: **`polylines`**

#### Polygon
<img align="right" src="assets/polygon.png" height="80">
- Google Maps: [google.maps.Polygon](https://developers.google.com/maps/documentation/javascript/reference#Polygon)
- Component property: **`polygons`**


## Controllers

### Array controllers
### Associated object controllers (item controllers)
### Customising the path to `lat`, `lng`, ...


## Views

### All views
### Creating an `InfoWindow` template
### Listening to Google events
### Accessing the Google map object (not recommended)