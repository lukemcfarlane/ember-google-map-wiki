### 1. Why does `{{google-map markers=m lat=m.firstObject.lat ...}}` fail?

Remember that the bindings used within this add on are 2-way. When you drag the map or zoom with the mouse wheel the center coordinates -- the ones at `lat` and `lng` -- will change. Then in our example the `lat` of the first marker will also be changed, resulting in an infinite loop while trying to synchronize bindings.

If you need to synchronize the center with the `lat` and `lng` of the first marker, an info-window, or any  overlay object, create a one way binding in your controller:

```js
  //...
  centerLat: Ember.computed.oneWay('markersArray.firstObject.lat'),
  centerLng: Ember.computed.oneWay('markersArray.firstObject.lng'),
  //...
```

and use that in your template instead:

```handlebars
{{google-map markers=markersArray lat=centerLat lng=centerLng ...}}
```

### 2. How can I handle Google events on overlays?

Refer to the [[Events|Provided-Tools-and-Classes-(API)#events]] section of the API and the [[Responding to Google events|Examples#responding-to-google-events]] section of the example.


### 3. I am receiving the `ERROR: markerViewClass must be a subclass or an instance of Ember.View` error. What's wrong?

Starting with version `0.0.14` of this add on and onward you will need Ember.js >= `1.9`. Please refer to the [[Requirements|Installation#requirements]] section of the installation docs.

### 4. Why am I'm getting errors in my console?

If you are using ember-cli you will need to setup [Content Security Policies](https://github.com/rwjblue/ember-cli-content-security-policy) allowing the correct domains.

Add the following security policy to your `config/environment.js` file to stop the warnings:

```javascript
  ENV.contentSecurityPolicy = {
    'default-src': "'none'",
    // Allow scripts
    'script-src': "'self' 'unsafe-eval' http://*.googleapis.com http://maps.gstatic.com",
    'font-src': "'self' http://fonts.gstatic.com", // Allow fonts
    'connect-src': "'self' http://maps.gstatic.com", // Allow data (ajax/websocket)
    'img-src': "'self' http://*.googleapis.com http://maps.gstatic.com http://csi.gstatic.com",
    // Allow inline styles and loaded CSS
    'style-src': "'self' 'unsafe-inline' http://fonts.googleapis.com http://maps.gstatic.com"
  };
````

### Have another question or issue? Ask me here by creating a new issue or contact me on twitter [@huafu_g](https://twitter.com/huafu_g).