### Introduction

It's not really a configuration, but a required step before you can ever see the map. As explained in the Google Maps documentation, the map requires that you define the size of the canvas. So you need to add one css rule:

```css
.map-canvas {
  width: 600px;
  height: 400px;
}
```

All configurations described below are done in the `config/environment.js` of your application, in a `googleMap` property of `ENV`:

```js
ENV.googleMap = {
  // your configuration goes here
}}
```

## Google API key or Client ID

* Google Map doesn't require an API key to be used, tho Google recommand to create and use one. To create a new API key, go [there](https://developers.google.com/maps/documentation/javascript/tutorial#api_key). Then you can use it by defining an **`apiKey`** property of the [[configuration|Configuration#introduction]].

* Using [Google Maps API for Work](https://developers.google.com/maps/documentation/business/)? Then you do not need an API key anymore, but a [client ID](https://developers.google.com/maps/documentation/business/clientside/#client_id). As for API key, you would put this one in the [[configuration|Configuration#introduction]], but in a **`clientId`** property.


## Lazy loading

By default, the Google Maps library will load automatically when your application starts. If you prefer to load it only the first time a map is required, you can then disable the auto-load by defining a **`lazyLoad`** property in the [[configuration|Configuration#introduction]] with value `true`.

You'll then need, in each route _(or parent, sub-parent, ... route)_ having a template using the `{{google-map}}` component, to load the library. To do so, a helper method **`loadGoogleMap`** has been injected in all routes:

```js
export default Ember.Route.extend({
  model: function (params) {
    return this.loadGoogleMap();
  }
});
```

It'll return a promise which will resolve to whatever you give as parameter. So if you need to still have your model loaded for example, do like this:

```js
export default Ember.Route.extend({
  model: function (params) {
    return this._super.apply(this, arguments).then(this.loadGoogleMap);
    // or without using the default ember-data but your own:
    return this.loadGoogleMap(theModelToResolveTo);
  }
});
```


## Loading additional Google libraries

Google Maps API provide the ability to load many more tools packaged as libraries. See the full list of them [there](https://developers.google.com/maps/documentation/javascript/libraries).

Adding additional libraries to be loaded with the Google Maps SDK is as simple as adding them to a **`libraries`** property of the [[configuration|Configuration#introduction]]. For example if you need to load the `places` and `geometry` libraries, to like this:

```js
  // ...
  libraries: ['places', 'geometry']
  // ...
```
