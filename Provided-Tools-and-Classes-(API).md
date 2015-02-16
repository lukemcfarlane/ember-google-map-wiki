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
- Google type: [google.maps.Marker](https://developers.google.com/maps/documentation/javascript/reference#Marker)
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
### Introduction

While you do not need to specify and extend any controller, you have the possibility to extend them to have full control over your map overlay objects. This allow you to have models for this objects which do not reflect the expected property names for example, or handle specific actions when a property value changes.

All controllers are injected in `app/controllers/google-map` path when the application is built. Let's say for example that you want to extend the `marker` controller in `app/controllers/my-marker.js`:

```js
import GoogleMapMarkerController from './google-map/marker';

export default GoogleMapMarkerController.extend({
  // ...
});
```

### Available controllers

All overlay object types have one controller which is used as the `itemController` for its associated array of objects. It is named as the object type and is extending `Ember.ObjectController`.

The `polylines` and `polygons` additionally have a path controller (`google-map/polyline-path` and `google-map/polygon-path`) which are extending `Ember.ArrayController` so that you can handle differently their paths.

Here is the list of all controllers available, all injected in `app/controllers/google-map` path:
- **`marker`**
- **`info-window`**
- **`circle`**
- **`polyline`**
- **`polyline-path`**
- **`polygon`**
- **`polygon-path`**

### Example use-case: custom `lat` and `lng` property names

Let's say that your marker model does not have `lat` and `lng` properties but instead a `coords` object with `latitude` and `longitude` properties. To make the component work properly, you'll need to:

1. extend the `google-map/marker` controller (we will do this in `app/controllers/map/my-marker.js)`:

    ```js
    import GoogleMapMarkerController from '../google-map/marker';

    export default GoogleMapMarkerController.extend({
      lat: Ember.computed.alias('coords.latitude'),
      lng: Ember.computed.alias('coords.longitude')
    });
    ```

2. inform the component that you want to use your own controller (in `app/templates/map.hbs`):

    ```handlebars
    {{google-map ... markerController='map/my-marker'}}
    ```

...and voila, now your records/objects will be used correctly and updated if the markers are draggable.


## Views

### Introduction

As for controllers, you do not need to extend and define the views to be used, but it can be useful if you need to listen for the click event for example.

All views are injected in `app/views/google-map` path when the application is built. Let's say for example that you want to extend the `marker` view in `app/views/my-marker.js`:

```js
import GoogleMapMarkerView from './google-map/marker';

export default GoogleMapMarkerView.extend({
  // ...
});
```

### Available views

All overlay object types have one view which is used to represent, even if only in-memory, each overlay object drawn on the map. It is named as the object type and is extending `Ember.View`.

Here is the list of all views available, all injected in `app/views/google-map` path:
- **`marker`**
- **`info-window`**
- **`circle`**
- **`polyline`**
- **`polygon`**

### Events

To listen for google events as specified in the header of each overlay type above, you need to extend the corresponding view and define what you want to do for each event type you want to handle on the `googleEvents` special property.

The `googleEvents` is a hash with each key being the name of the event to listen, and the value being the name of the action to send in case this event is triggered. You can also define each event as an object instead of a string, in which case it has the following options:

- **`target`** (`Object`): the target used to send the action or context of the method to call
- **`action`** (`string`): the name of the action to send
- **`method`** (`string` or `Function`): the name or method to call, ignored if `action` is defined, default to the name of the event

### Example use-case: responding to events

Let's say you want to handle the `click` event on your circles to send the `didClick` Ember action (as if `{{action 'didClick'}}`) was thrown from a template. To achieve this you'll need to:

1. create your own circle view extending the one of this addon and define the event (we will do this in `app/views/map/my-circle.js`):

    ```js
    import GoogleMapCircleView from '../google-map/circle';

    export default GoogleMapCircleView.extend({
      googleEvents: {
        click: 'didClick'
      }
    });
    ```

2. tell the component that you want to use a specific view for each circle (in `app/templates/map.hbs` for example):

    ```handlebars
    {{google-map ... circleViewClass='map/my-circle'}}
    ```

...and the `didClick` action will be sent when a circle is clicked.

### Accessing the Google map object (not recommended)

**This is strongly not recommended**, but if you really need to access the Google Maps core `map` object, you can bind the `map` property to your view, then it'll be accessible from that view:

```handlebars
{{google-map ... map=view.googleMap}}
```

In the view you can then access the `googleMap` object and it'll be the instance of `google.maps.Map` used to handle the map.
