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

## Configuring your Content Security Policy
In order to avoid errors in the console, you will need to setup [Content Security Policies](https://github.com/rwjblue/ember-cli-content-security-policy) allowing the correct domains.

Add the following security policy to your `config/environment.js` file:

```javascript
  ENV.contentSecurityPolicy = {
    'default-src': "'none'",
    'script-src': "'self' 'unsafe-eval' *.googleapis.com maps.gstatic.com",
    'font-src': "'self' fonts.gstatic.com",
    'connect-src': "'self' maps.gstatic.com",
    'img-src': "'self' *.googleapis.com maps.gstatic.com csi.gstatic.com",
    'style-src': "'self' 'unsafe-inline' fonts.googleapis.com maps.gstatic.com"
  };
````

## Google API version
* By default, the component uses version 3 of the API, but you may select a specific revision with the `version` property.

## Google API key or Client ID

* Using the Google Maps API doesn't require an API key, but Google recommends creating and using one. To create a new API key, go to [this link](https://developers.google.com/maps/documentation/javascript/tutorial#api_key). Once you have obtained an API key you will be able to use it by defining an **`apiKey`** property within the [[configuration|Configuration#introduction]].

* If you are using the [Google Maps API for Work](https://developers.google.com/maps/documentation/business/) then you no longer need an API key. Instead you will need a [client ID](https://developers.google.com/maps/documentation/business/clientside/#client_id). You will need to add your clientId to the [[configuration|Configuration#introduction]] by defining the **`clientId`** property.

## Channel

To provide additional detail in your reports, you can optionally specify a [channel](https://developers.google.com/maps/documentation/business/clientside/load#channel_reporting).

#### Example:

```js
ENV.googleMap = {
  clientId: 'clientid',
  channel: 'my-app-name'
}}
```

## Lazy loading

By default, the Google Maps library will load automatically when your application starts. If you prefer to defer loading it until the first time a map is required, then you can disable auto-loading by defining a **`lazyLoad`** property in the [[configuration|Configuration#introduction]] and setting **`lazyLoad`** to  `true`.

If you are using lazy-loading then you'll need to ensure that the Google Maps library is loaded for each route  that has a template using the `{{google-map}}` component. A helper method **`loadGoogleMap`** has been injected in all routes so that you can load the library:

```js
export default Ember.Route.extend({
  model: function (params) {
    return this.loadGoogleMap();
  }
});
```

The **`loadGoogleMap`** method will return a promise which will resolve to whatever you give as parameter. So, for example, if you need to load the Google Maps library and still have your model loaded, you can do the following:

```js
export default Ember.Route.extend({
  model: function (params) {
    return this._super.apply(this, arguments).then(this.loadGoogleMap);
    // or without using the default ember-data but your own:
    return this.loadGoogleMap(theModelToResolveTo);
  }
});
```

The Google Maps library can be lazy-loaded in a parent of the route using the map component. For instance, if you have a `user.travels.map` route, you can lazy load the Google Maps library in the `user.travels.map` route, `users.travels` route or `users` route. This is useful in scenarios where you would otherwise have to lazy-load the library in multiple places. If you have the routes `users.travels.map` and `users.travel.all`, for instance, you can lazy load the library in the `user.travels` route alone.

## Loading additional Google libraries

The Google Maps API provides the ability to load many more tools packaged as libraries. See the full list of libraries [here](https://developers.google.com/maps/documentation/javascript/libraries).

Adding additional libraries to be loaded alongside the Google Maps SDK is as simple as adding them to the **`libraries`** property of the [[configuration|Configuration#introduction]]. For example if you need to load the `places` and `geometry` libraries as well, add them to the **`libraries`** property as follows:

```js
  // ...
  libraries: ['places', 'geometry']
  // ...
```
