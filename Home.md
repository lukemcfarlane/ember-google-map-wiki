![EmberJS Google Map addon](https://raw.githubusercontent.com/huafu/ember-google-map/master/icon-64.png) EmberJS Google Map help index
=============================

[![NPM](https://nodei.co/npm/ember-google-map.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/ember-google-map/)

#### [[Installation|install]]
* [[Requirements|install#requirements]]
* [[Install the addon|install#install-the-addon]]

#### [[Configuration|config]]
* [[Google API key or Client ID|config#google-api-key-or-client-id]]
* [[Lazy loading|config#lazy-loading]]
* [[Loading additional Google libraries|config#loading-additional-google-libraries]]

#### [[Provided Tools and Classes (API)|api]]
* Main `{{google-map}}` component
    - Basic properties
    - Google options as properties
    - Defining objects to draw
* Controllers
    - Array controllers
    - Associated object controllers (item controllers)
    - Customising the path to `lat`, `lng`, ...
* Views
    - All views
    - Creating an `InfoWindow` template
    - Listening to Google events
    - Accessing the Google map object (not recommended)

#### [[Examples|examples]]
* Basic: map with a given zoom and position
* Adding markers with info-windows
* Adding poly-lines
* Listening and answering Google events

#### [[FAQ|faq]]
* Why `{{google-map markers=m lat=m.firstObject.lat...}}` fails?
* _complete with bugs that have been reported so far_

#### [[About|about]]
* Author
* Contributors