## Basic map with a given zoom and position

1. **Create a new Ember application (supposing here that you already have installed `ember-cli`) and install this addon. In a terminal, type:**

    ```bash
    ember new google-map-demo
    cd google-map-demo
    ember install:addon ember-google-map
    ```

    then follow the steps [there](http://emberjs.com/blog/2014/12/08/ember-1-9-0-released.html#toc_handlebars-2-0) to update your Handlebars version, and update your bower `ember` package to 1.9.1 or up with:

    ```bash
    bower install --save ember#1.9.1
    ```

2. **Go edit the application template (at `app/templates/application.hbs`) and replace it with:**

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

3. **Create the `application` controller. In the terminal, type:**

    ```bash
    ember g controller application
    ```

    and complete the `app/controllers/application.js` so that it looks like this:

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

5. **Run the application and checkout the result. In the terminal, type:**

    ```bash
    ember serve
    ```

    and open http://localhost:4200 with your preferred browser.

## Adding markers with info-windows

Now it's time to add some overlay objects in your map. We'll create a fake model representing our markers and use them in the component.

1. **Create the application route so that we can return a fake model representing the markers. In the terminal, type** (when asking to overwrite `app/templates/application.hbs` answer `n`):

    ```bash
    ember g route application
    ```

    and edit `app/routes/application.js` so that it looks like this:

    ```js
    import Ember from 'ember';

    export default Ember.Route.extend({
      model: function() {
        return [
          {title: "Home", lat: 14.766127, lng: 102.810987, body: "Here is B&H's home"},
          {title: "Shop", lat: 14.762963, lng: 102.812285, body: "Here is B&H's shop", isInfoWindowVisible: true},
          {title: "Hay's", lat: 14.762900, lng: 102.812018, body: "Here is Hay's shop"}
        ];
      }
    });
    ```

2. **Create the info-window template. In the terminal, type:**

    ```bash
    ember g template map/info-window
    ```

    and then go edit `app/templates/map/info-window.hbs` so that it looks like this:

    ```handlebars
    <strong>{{title}}</strong>
    <p>{{body}}</p>
    <p>
      Coordinates:<br>
      <small>
        <code>{{lat}}</code>,<code>{{lng}}</code>
      </small>
    </p>
    ```

3. **Now we need to update the `application` template so that we tell the component to use our own template. Edit `app/templates/application.hbs` and add the `markerInfoWindowTemplateName` and `markers` properties in `{{google-map ...}}`:**

    ```handlebars
    {{google-map markerInfoWindowTemplateName='map/info-window'
                 markers=model
                 ...}}
    ```

4. **Start `ember serve` if you had stopped it, else just go to http://localhost:4200 and check the result**

## Adding polylines

As we are lazy (well, I am), we are going to use the 3 existing markers as the path for our polyline, but you of course can use data from another source or create another fake model to test.

We also want to have the polyline being editable, so that you can see the powerfulness of the data binding coupled with Google Maps events made easy thanks to this addon. Changing a path's item (white spot) on the map will also move the marker since we'll use the same data. It'll also add another marker if you add a new point in the path by dragging the faded white spots of the polyline.

1. **Open the `application` controller so that we can create the property holding our polylines, actually here only one. Edit `app/controllers/application.js` and add these properties:**

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

2. **Now go edit the `application` template to set the `polylines` property of the `{{google-map...}}` tag:**

    ```handlebars
    {{google-map polylines=polylinesArray
                 ...}}
    ```

4. **Finally start `ember serve` if you had stopped it, else just go to http://localhost:4200 and to check it out**

You could also add `{{polylinesArray.firstObject.path.length}}` in the `application` template to follow the number of points making the polyline's path, and look at it changing when you create more edges by dragging the faded white spots in the middle of existing edges on the map.

## Answering Google events
