### 1. Why `{{google-map markers=m lat=m.firstObject.lat ...}}` fails?

Remember that bindings are 2-way bindings, so when you're going to drag the map or zoom with the mouse wheel, the center coordinates, ie the ones at `lat` and `lng`, will change, and so it'll change here the `lat` of the first marker too, ending in an infinite loop trying to synchronize bindings.

If you need to synchronize the center with the `lat` and `lng` of the first marker, or info-window, or whatever overlay object, creating a one way binding in your controller:

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

Refer the [[Events|Provided-Tools-and-Classes-(API)#events]] section of the API or even the [[Answering Google events|Examples#answering-google-events]] section of the example.


### 3. Getting `ERROR: markerViewClass must be a subclass or an instance of Ember.View` error, what's wrong?

Since 0.0.14 you'll need EmberJS >= `1.9`. Refer to the [[Requirements|Installation#requirements]] section of the installation docs.


### Having another question/issue? Ask me here creating a new issue or on twitter [@huafu_g](https://twitter.com/huafu_g).
