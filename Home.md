<img src="https://nodei.co/npm/ember-google-map.png?downloadRank=true&stars=true">
<img src="wiki/assets/icon.png" align="right" width="150" height="150">


EmberJS Google Map
==================

#### [[Installation]]
* [[Requirements|Installation#requirements]]
* [[Install the addon|Installation#install-the-addon]]

#### [[Configuration]]
* [[Google API key or Client ID|Configuration#google-api-key-or-client-id]]
* [[Lazy loading|Configuration#lazy-loading]]
* [[Loading additional Google libraries|Configuration#loading-additional-google-libraries]]

#### [[Provided Tools and Classes (API)]]
* [[Main `{{google-map}}` component|Provided-Tools-and-Classes-(API)#main-google-map-component]]
    - Basic properties
    - Google options as properties
    - Defining objects to draw
        - markers
        - info-windows
        - circles
        - polylines
        - polygons
* [[Controllers|Provided-Tools-and-Classes-(API)#controllers]]
    - Introduction
    - Available controllers
    - Example use-case: custom `lat` and `lng` property names
* [[Views|Provided-Tools-and-Classes-(API)#views]]
    - Introduction
    - Available views
    - Events
    - Example use-case: responding to events
    - Accessing the Google map object (not recommended)

#### [[Examples]]
* Basic map with a given zoom and position
* Adding markers with info-windows
* Adding poly-lines
* Listening and answering Google events

#### [[FAQ]]
* Why `{{google-map markers=m lat=m.firstObject.lat...}}` fails?
* _complete with bugs that have been reported so far_

#### [[About]]
* Author
* Contributors
