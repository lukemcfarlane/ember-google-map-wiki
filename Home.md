<img src="https://nodei.co/npm/ember-google-map.png?downloadRank=true&stars=true&downloads=true">
<img src="wiki/assets/icon.png" align="right" width="150" height="150">


EmberJS Google Map
==================

#### [[Installation]]
* [[Introduction|Installation#introduction]]
* [[Requirements|Installation#requirements]]
* [[Install the addon|Installation#install-the-addon]]

#### [[Configuration]]
* [[Content Security Policy|Configuration#configuring-your-content-security-policy]]
* [[Google API key or Client ID|Configuration#google-api-key-or-client-id]]
* [[Lazy loading|Configuration#lazy-loading]]
* [[Loading additional Google libraries|Configuration#loading-additional-google-libraries]]

#### [[Provided Tools and Classes (API)]]
* [[The main {{google-map}} component|Provided-Tools-and-Classes-(API)#the-main-google-map-component]]
    - [[Basic properties|Provided-Tools-and-Classes-(API)#basic-properties]]
    - [[Google options as properties|Provided-Tools-and-Classes-(API)#google-options-as-properties]]
    - [[Defining objects to draw|Provided-Tools-and-Classes-(API)#defining-objects-to-draw]]
        - [[markers|Provided-Tools-and-Classes-(API)#markers]]
        - [[info-windows|Provided-Tools-and-Classes-(API)#info-windows]]
        - [[circles|Provided-Tools-and-Classes-(API)#circles]]
        - [[polylines|Provided-Tools-and-Classes-(API)#polylines]]
        - [[polygons|Provided-Tools-and-Classes-(API)#polygons]]
* [[Controllers|Provided-Tools-and-Classes-(API)#controllers]]
    - [[Introduction|Provided-Tools-and-Classes-(API)#introduction-1]]
    - [[Available controllers|Provided-Tools-and-Classes-(API)#available-controllers]]
    - [[Example use-case: custom `lat` and `lng` property names|Provided-Tools-and-Classes-(API)#example-use-case-custom-lat-and-lng-property-names]]
* [[Views|Provided-Tools-and-Classes-(API)#views]]
    - [[Introduction|Provided-Tools-and-Classes-(API)#introduction-2]]
    - [[Available views|Provided-Tools-and-Classes-(API)#available-views]]
    - [[Events|Provided-Tools-and-Classes-(API)#events]]
    - [[Example use-case: responding to events|Provided-Tools-and-Classes-(API)#example-use-case-responding-to-events]]
    - [[Styling|Provided-Tools-and-Classes-(API)#styling]]
    - [[Accessing the Google map object (not recommended)|Provided-Tools-and-Classes-(API)#accessing-the-google-map-object]]

#### [[Examples]]
* [[Basic map with a given zoom and position|Examples#basic-map-with-a-given-zoom-and-position]]
* [[Adding markers with info-windows|Examples#adding-markers-with-info-windows]]
* [[Adding polylines|Examples#adding-polylines]]
* [[Answering Google events|Examples#answering-google-events]]

#### [[FAQ]]
* [[Why {{google-map markers=m lat=m.firstObject.lat ...}} fails?|FAQ#1-why-google-map-markersm-latmfirstobjectlat--fails]]
* [[How can I handle Google events on overlays?|FAQ#2-how-can-i-handle-google-events-on-overlays]]
* [[Getting ERROR: markerViewClass must be a subclass or an instance of Ember.View error, what's wrong?|FAQ#3-getting-error-markerviewclass-must-be-a-subclass-or-an-instance-of-emberview-error-whats-wrong]]

#### [[About]]
* [[Author|About#author]]
* [[Contributors|About#contributors]]