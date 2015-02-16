## Basic map with a given zoom and position

1. Create a new Ember application (supposing here that you already have installed `ember-cli`). In a terminal, type:

    ```bash
    ember new google-map-demo
    cd google-map-demo
    ```

    then follow the steps [there](http://emberjs.com/blog/2014/12/08/ember-1-9-0-released.html#toc_handlebars-2-0) to update your Handlebars version, and update your bower `ember` package to 1.9.1 or up with:

    ```bash
    bower install --save ember#1.9.1
    ```

2. Go edit the application template (at `app/templates/application.hbs`) and replace it with:

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

3. Create the `application` controller. In the terminal, type:

    ```bash
    ember g controller application
    ```

    and complete the `app/controllers/application.js` so that it looks like this:

    ```js
    import Ember from 'ember';

    export default Ember.ArrayController.extend({
      zoom: 17,
      centerLat: 14.7643531,
      centerLng: 102.8115874
    });
    ```

4. Edit `app/styles/app.css` to define the size of the map canvas:

    ```css
    .map-canvas {
      width: 600px;
      height: 400px;
    }
    ```

5. Run the application and checkout the result. In the terminal, type:

    ```bash
    ember serve
    ```

    and open http://localhost:4200 with your preferred browser.

## Adding markers with info-windows

Now it's time to add some overlay objects in your map. We'll create a fake model representing our markers and use them in the component.

1. Create the application route so that we can return a fake model representing the markers. In the terminal, type:

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
          {title: "Shop", lat: 14.762963, lng: 102.812285, body: "Here is B&H's shop!"}
        ];
      }
    });
    ```

2. Create the info-window template. In the terminal, type:

    ```bash
    ember g template map/info-window
    ```

    and then go edit `app/templates/map/info-window.hbs` so that it looks like this:

    ```handlebars
    <h4>{{title}}</h4>
    <p>{{body}}</p>
    <p>
      Coordinates: <br>
      lat: <small><pre>{{lat}}</pre></small>
      lng: <small><pre>{{lng}}</pre></small>
    </p>
    ```

3. Now we need to update the `application` template so that we tell the component to use our fresh template. Edit `app/templates/application.hbs` and add the `markerInfoWindowTemplateName` property in `{{google-map ...}}`:

    ```handlebars
    {! ... }}
    {{google-map markerInfoWindowTemplateName='map/info-window'
                 ...}}
    {! ... }}
    ```

4. Start `ember serve` if you had stopped it, else just go to http://localhost:4200 and check the result.

## Adding polylines

## Answering Google events
