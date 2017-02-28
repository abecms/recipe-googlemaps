# Google Maps Abe Recipe
This Abe recipe demonstrates how to add a Google Maps field to your template and display a Google Map

![Screenshot](/site/screenshot.png?raw=true)

# Installation
1. git clone https://github.com/Abejs/recipe-googlemaps.git
2. cd recipe-googlemaps
3. abe serve
4. open your browser at the address : http://localhost:3000/abe/editor
5. Enjoy !

# Description
In this demo, you'll see 4 recipes:
1. GoogleMaps - a Google Maps autocomplete field in the editor + a Google Map filled with geolocations in the template
2. Storelocator ws - A storelocator grabbing data in json format from a remote server
3. Store management - How to create and maintain a stores json file from Abe
4. Storelocator ws - The storelocator now uses your stores file created in the previous recipe

# Google Maps
## The autocomplete field
``` 
{{abe type='data' key='gmaps' source="https://maps.googleapis.com/maps/api/geocode/json?key=YOURKEY&address=" autocomplete="true" display="{{formatted_address}} - (lat:{{geometry.location.lat}}-lng:{{geometry.location.lng}})" desc='gmaps'}}
```
The Abe type used is a data type. In the source attribute, put the url of Gmaps geocode search with the "address" querystring empty. It will be filled in with your autocomplete data. 
Add the attribute autocomplete="true" and chose the data extracted from the gmaps json you want to display to the user. In the example above, we illustrate the possibility to add different fields: {{formatted_address}}, {{geometry.location.lat}} and {{geometry.location.lng}}

That's all : With this simple Abe tag, your users will enjoy the Google geolocation feature directly from Abe editor!
The data selected by the user will be saved with the document.

## Display a Google Map in the template
The code sample :
```
<div id="map"></div>
{{#if gmaps.0.formatted_address}}
    <script>
      function initMap() {
	var map = new google.maps.Map(document.getElementById('map'), {
	  zoom: 4,
	  center: new google.maps.LatLng(
		{{gmaps.0.geometry.location.lat}},
		{{gmaps.0.geometry.location.lng}}
	  )
	});
	{{#each gmaps}}
	new google.maps.Marker({
	  position: new google.maps.LatLng(
		{{geometry.location.lat}},
		{{geometry.location.lng}}
	  ),
	  map: map
	});
	{{/each}}
      }
    </script>
    <script async defer src="https://maps.googleapis.com/maps/api/js?key=YOURKEY&callback=initMap"></script>
{{/if}}
```
The Gmaps code has been taken from the Google Maps site dev site. In this sample, we don't display the map if there is no record to display ({{#if gmaps.0.formatted_address}})
Then we center the map on the first record : 
``` 
  center: new google.maps.LatLng(
    {{gmaps.0.geometry.location.lat}},
    {{gmaps.0.geometry.location.lng}}
  ) 
```
Finally, we parse each record and create a Marker:
```
{{#each gmaps}}
  new google.maps.Marker({
    position: new google.maps.LatLng(
      {{geometry.location.lat}},
      {{geometry.location.lng}}
    ),
    map: map
  });
{{/each}}
```	
That's all (don't forget to put your own key: The key in the sample code only works for localhost requests and should be limited to your first test ;)

# Storelocator WS

# Store management (template : store.html)

## Description
using the variable ```abeEditor``` you can know if you display your template in the editor or not. We use this variable here to present properly (in HTML) our json to the contributor. Once the contributor publish her stores, it will be published as a json file (for now, the extension of the file remains .html but its content is a pure json).
This set of stores is easily maintained by your contributors: During the creation of a store, we show you how it's possible to use a ws (GoogleMaps) to search for a location, and add additional fields to your store (open hours, image, whatever...)

# Storelocator WS with store.html

## Description
This time, the storelocator uses data from your own stores file. Just change the source of the data:

```
dataLocation           : '/store-management.html'
```

