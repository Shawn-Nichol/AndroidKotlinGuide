 Google maps requires an API key. To obtain the API key, you register your project in the Google API Console. The API key is tied toa digital certificate that links the app to its author.
 
 Debug certificate is insecure by design, and described in sign your debug build. Published android apps that use the Google Maps API require a second API key: the key for the release certificate. 
 
 Google maps include several types of maps, Normal, Satellite, Hybrid, terrain, and none. 
 
 Zoom level
1: World
5: Landmass/continent
10: City
15: Streets
20: Buildings


## Summary
- To use the Maps API, you need a key from the Googleapi Console
- In Android studio, using the Google Maps Activity template generates an Activity with a single SupportMapFragment in teh app's layout. The template also adds the ACCESS_FINE_PERMISSION to the app manifest, and implements the OnMapReadycallback in your activity, and overrides the required onMapRead()

To change the map type of a GoogleMap at runtime, use the GoogleMap.setMapType(). A google Map can be one of the following mapt types
- Normal: Typical road map. Shows roads, some features built by humans , and important natural features like reivers. Roads and feature labels are also visible. 
- Hybrid: Satellite photograph data with road maps added. Road and feature labels are also visible
- Satellite: Photograph data. Road and feature labels are not visible. 
- Terrain: Topgraphic data the Map includes colors contour lines and labels and perspective shading. Some roads and labels are also visible 
- Non; No base map tiles

Google Maps
- A marker is an indicator for a specific geographic location
- When tapped, the marker's default behavior is dto display an info window with the information about the location. 
- By default, point of interest(POIs) appear on the base map along with their corresponding icons. POIs include parks, schools, goverment buildings and more. 
- In addition, buiness POIs(Shops, reestaurants, hotels and more) appear by default on the map when the map type is normal
- You an capture clicks on POIs using the OnPoiClickListener
- You can change the visual appearance of almost all elements of a Google Map using the setMapStyle(). 
- You can customize your markers by changeing the default color, or replacing the default marker icon with a custom image. 

Other important information
- Use a ground overlay to fix an image to a geographic location
- Uses a GroundOverlayoutOPtions object specify the image, the image's size in meters, and the image's
- Provided that your app has the ACCESS_FINE_LOCATION permission, you can enable location tracking by setting map. isMyLocationEnabled = tru
- You can also provide additional infomation about a location
