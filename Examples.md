## Basic map with a given zoom and position

1. **Create a new Ember application (assuming that you already have installed `ember-cli`) and install this addon. In a terminal, type:**

    ```bash
    ember new google-map-demo
    cd google-map-demo
    ember install ember-google-map
    ```

    then follow the steps [here](http://emberjs.com/blog/2014/12/08/ember-1-9-0-released.html#toc_handlebars-2-0) to update your version of Handlebars. Update your bower `ember` package to 1.9.1 or above with:

    ```bash
    bower install --save ember#1.9.1
    ```

2. **Edit the application template (at `app/templates/application.hbs`) and replace it with:**

    ```handlebars
    <h1>Welcome to Google Map demo!</h1>

    {{google-map lat=centerLat
                 lng=centerLng
                 zoom=zoom
                 gopt_zoomControl=false
                 type='satellite'}}

    <div class="controls">
      <label>
        Zoom:
        {{input type="range" value=zoom min=0 max=18 step=1}}
      </label>
    </div>
    ```

3. **Create the `application` controller using `ember-cli`. In the terminal, type:**

    ```bash
    ember g controller application
    ```

    and finally modify `app/controllers/application.js` so that it looks like this:

    ```js
    import Ember from 'ember';

    export default Ember.ArrayController.extend({
      zoom: 17,
      centerLat: 14.7646531,
      centerLng: 102.8115874
    });
    ```

4. **Edit `app/styles/app.css` to define the size of the map canvas:**

    ```css
    .map-canvas {
      width: 600px;
      height: 400px;
    }
    ```

5. **Run the application and checkout the result of your work. In the terminal, within the folder containing your application, type:**

    ```bash
    ember serve
    ```

    and open `http://localhost:4200` with your preferred browser.

## Adding markers with info-windows

Now it's time to add some overlay objects to your map. We'll create a fake model representing our markers and use them in the component.

1. **Create the application route so that we can return a fake model representing the markers. In the terminal, type** (when asked to overwrite `app/templates/application.hbs` answer `n`):

    ```bash
    ember g route application
    ```

    and edit `app/routes/application.js` so that it looks like this:

    ```js
    import Ember from 'ember';

    export default Ember.Route.extend({
      model: function() {
        return Ember.A([
          {title: "Home", lat: 14.766127, lng: 102.810987, body: "Here is B&H's home"},
          {title: "Shop", lat: 14.762963, lng: 102.812285, body: "Here is B&H's shop", isInfoWindowVisible: true},
          {title: "Hay's", lat: 14.762900, lng: 102.812018, body: "Here is Hay's shop"}
        ]);
      }
    });
    ```

2. **Create the info-window template. In the terminal, type:**

    ```bash
    ember g template map/info-window
    ```

    and then edit `app/templates/map/info-window.hbs` so that it looks like this:

    ```handlebars
    <strong>{{model.title}}</strong>
    <p>{{model.body}}</p>
    <p>
      Coordinates:<br>
      <small>
        <code>{{model.lat}}</code>,<code>{{model.lng}}</code>
      </small>
    </p>
    ```

3. **Now we need to update the `application` template so that we tell the component to use the template we just created. Edit `app/templates/application.hbs` and add the `markerInfoWindowTemplateName` and `markers` properties in `{{google-map ...}}`:**

    ```handlebars
    {{google-map markerInfoWindowTemplateName='map/info-window'
                 markers=model
                 ...}}
    ```

4. **Start `ember serve` if you had stopped it, otherwise go to http://localhost:4200 and check the result of the modifications we made**

## Adding polylines

Since we are all lazy (well, I am), we are going to use the 3 existing markers as the path for our polyline. Of course you can can use data from another source or create a fake model to perform this test with your own data.

We also want to make the polyline editable, so that you can see the power of data binding along with Google Map events that are made easy thanks to this addon. Changing the location of a path item (white spot) on the map will also move the marker since we are using the same data. Another marker will also be added if you add a new point to the path by dragging the faded white spots of the polyline.

1. **Open the `application` controller so that we can create the property which will hold our polylines. In this case we only have a single line. Edit `app/controllers/application.js` and add these properties:**

    ```js
      polyline: Ember.computed('model', function() {
        return {
          path:       this.get('model'),
          isEditable: true
        };
      }),

      polylinesArray: Ember.computed('polyline', function() {
        return [ this.get('polyline') ];
      })
    ```

2. **Now edit the `application` template to set the `polylines` property of the `{{google-map...}}` tag:**

    ```handlebars
    {{google-map polylines=polylinesArray
                 ...}}
    ```

4. **Finally, start `ember serve` if you had stopped it. Navigate to http://localhost:4200 in your browser to check out what we have built**

You can also add `{{polylinesArray.firstObject.path.length}}` to the `application` template in order to track the number of points composing the polyline's path. Watch it change as you create more edges by dragging the faded white spots in between the existing edges on the map.


## Responding to Google events

Now we will set up functionality to duplicate a marker when the user right clicks on it. In order to implement this functionality, we need to create and extend the default marker view so that we can listen to the `rightclick` event.

1. **Create our extended marker view so we can listen to events. In the terminal, type:**

    ```bash
    ember g view map/marker
    ```

    then edit `app/views/marker.js` so that it looks like this:

    ```js
    import GoogleMapMarkerView from '../google-map/marker';

    export default GoogleMapMarkerView.extend({
      googleEvents: { rightclick: 'duplicateMarker' }
    });
    ```

2. **Add the `markerViewClass` property to the `application` template within the `{{google-map...}}` tag:**

    ```handlebars
    {{google-maps markerViewClass='map/marker'
                  ...}}
    ```

3. **Create the `duplicateMarker` action in the `application` controller. Add this at the end of `app/controllers/application.js`:**

    ```js
      actions: {
        duplicateMarker: function (target) {
          var newMarker, marker;
          // the ember target is the view, so our marker is the model of the controller for that view
          marker = target.get('controller.model');
          // we copy the marker
          newMarker = Ember.getProperties(marker, Ember.keys(marker));
          // add 0.0002 to its coordinates
          newMarker.lat += 0.0002;
          newMarker.lng += 0.0002;
          // and insert it right before the copied marker
          this.insertAt(this.indexOf(marker) + 1, newMarker);
          // stop propagation so that the context menu doesn't show-up
          return false;
        }
      }
    ```

4. **Finally, start `ember serve` if you stopped it. Navigate to http://localhost:4200 and check out the way the map behaves when responding to Google events.**
