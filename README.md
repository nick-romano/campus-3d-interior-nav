# campus-3d-interior-nav
<img src="https://github.com/nick-romano/campus-3d-interior-nav/blob/master/imgs/App.PNG" />
project outline for 3d interior navigation on the UMD campus, no code shared at the moment.


<h3>Background: </h3>
This project is focused on taking 2D floorplans of campus building and creating and interior navigation network that can be used online. The project is heavily based on the ArcGIS indoors methods and schema. 

<h3>Data:</h3>

Data was assimilated from CAD files provided from campus planners into the ESRI Local Government information model. When available, existing GIS data was used within the project. Python is used to automate this process into a repeatable workflow. 


<h3>Source map</h3>

| Source  | LGIM Feature class |
| ------- | ------------- |
| Building Footprints  | Buildings  |
| Building Footprints  | Building Floors  |
| CAD Walls Layer | BuildingFloorplanLine |
| CAD Glass Layer | BuildingFloorplanLine |
| CAD Doors Layer | BuildingFloorplanLine |
| CAD Room Polygon Layer (FICM) | BuildingInteriorSpace |
| Room Centriods (GIS) | BuildingInteriorSpacePt |




<h3>Process</h3>

The ArcGIS Indoors methods can be used to create the interior networks, the work comes in the form of getting the data into the LGIM schema, once inside LGIM, the tools work well.

<h4>Things to note- <h4>

* Make sure to use the rotation field if the building isn't in a north-south orientation
* It's better to use a meter based vertical coordinate system, but if you must use feet, be sure to convert it to meters before publishing to ArcGIS Online
* You can't create network datasets in ArcGIS Pro, but you can in ArcGIS Catalog- I had more success taking the pathways and floor transistions, and creating a new network dataset out of them, than using the output of the scripts.



<h3>Visualization:</h3>


<h4>Basement:</h4>
Visualizing the basement of a building in an online webscene is a novel issue. Because a basement is below the earths natural elevation, the default world elevation surface will be above the basement layer, and the basement will not be visible in the web scene. To make the basement visible, we had to alter a DEM and drop the surface elevation below the ground at the building's location. The DEM must then be uploaded as a custom elevation surface, and loaded into the scene.

Before:
<img src="https://github.com/nick-romano/campus-3d-interior-nav/blob/master/imgs/basement_before.PNG" style="border: 1px solid black"/>

<br>
After:
<img src="https://github.com/nick-romano/campus-3d-interior-nav/blob/master/imgs/basement_after.PNG" />

<h4>Routes:</h4>
Managing vertical Datums in the 4.6 API is still pretty difficult. We want to keep our network data in feet- but the current scene view only seems to reconize z-elevations as meters. So our current workflow is to develop data in a feet, and publish the Network dataset in feet. The scene viewer will read the feet data and display it as meters somewhere deep in the ether of the scene viewer code- I have no control over that. When we send stops to the routing service, the z-data for each stop must be altered and converted to feet from meters. The same proccess occurs when we recieve the route as z-feet back from the server, we convert the route back to z-meter.



