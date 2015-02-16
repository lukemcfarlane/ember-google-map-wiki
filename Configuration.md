### Introduction

As explained in the Google Maps documentation, before you can see the map you must define the size of the canvas in which the map is displayed. You need to add one css rule which sets the height and width of the canvas:

```css
.map-canvas {
  width: 600px;
  height: 400px;
}
```

All configuration options described below must be defined in the `config/environment.js` of your application, in a `googleMap` property of `ENV`:

```js
ENV.googleMap = {
  // your configuration goes here
}}
```

## Google API key or Client ID

* Using the Google Maps API doesn't require an API key, but Google recommends creating and using one. To create a new API key, go to [this link](https://developers.google.com/maps/documentation/javascript/tutorial#api_key). Once you have obtained an API key you will be able to use it by defining an **`apiKey`** property within the [[configuration|Configuration#introduction]].

* If you are using the [Google Maps API for Work](https://developers.google.com/maps/documentation/business/) then you no longer need an API key. Instead you will need a [client ID](https://developers.google.com/maps/documentation/business/clientside/#client_id). You will need to add your clientId to the [[configuration|Configuration#introduction]] by defining the **`clientId`** property.


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
