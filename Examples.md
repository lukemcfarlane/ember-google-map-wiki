## Basic map with a given zoom and position

1. Create a new Ember application (supposing here that you already have installed `ember-cli`). In a terminal, type:

    ```bash
    ember new google-map-demo
    cd google-map-demo
    ```

2. Go edit the application template (at `app/templates/application.hbs`) and replace it with:

    ```handlebars
    <h1>Welcome to Google Map demo!</h1>

    {{google-map lat=centerLat
                 lng=centerLng
                 zoom=zoom
                 gopt_zoomControl=false}}

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

    export default Ember.Controller.extend({
      zoom: 15,
      centerLat: 14.7634505,
      centerLng: 102.8124135
    });
    ```

4. Run the application and cehckout the result. In the terminal, type:

    ```bash
    ember serve
    ```

    and open http://localhost:4200 with your prefered browser.

## Adding markers with info-windows

## Adding polylines

## Answering Google events