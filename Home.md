![EmberJS Google Map addon](https://raw.githubusercontent.com/huafu/ember-google-map/master/icon-64.png) EmberJS Google Map help index
=============================


#### Installation

#### Configuration
* Google API key or Client ID
* Lazy loading
* Loading additional Google libraries

#### Provided Tools and Classes
* Main `{{google-map}}` component
    - Basic properties
    - Google options as properties
    - Defining objects to draw
* Controllers
    - Array controllers
    - Associated object controllers (item controllers)
    - Customizing the path to `lat`, `lng`, ...
* Views
    - All views
    - Creating an `InfoWindow` template
    - Listening to Google events
    - Accessing the Google map object (not recommended)

#### Examples
* Basic: map with a given zoom and position
* Adding markers with info-windows
* Adding polylines
* Listening and answering Google events

#### FAQ
* Why `{{google-map markers=m lat=m.firstObject.lat...}}` fails?
* _complete with bugs that have been reported so far_