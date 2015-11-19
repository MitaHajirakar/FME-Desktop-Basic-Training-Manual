<table style="border-spacing: 0px">
<tr>
<th style="background-color:darkorange;color:white">Exercise 4a</th>
<th style="background-color:darkorange;color:white">Readers</th>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Scenario</td>
<td style="border: 1px solid darkorange">FME Workspace Author</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Data</td>
<td style="border: 1px solid darkorange">Property Parcels (AutoCAD DWG/PostGIS)<br>Public Art (Microsoft Excel)<br>City Properties (Esri Shapefile)</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Overall Goal</td>
<td style="border: 1px solid darkorange">Create a list of public art on city property</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Demonstrates</td>
<td style="border: 1px solid darkorange">Managing and controlling Readers and Writers</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Start Workspace</td>
<td style="border: 1px solid darkorange">None</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">End Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4a-Complete.fmw</td>
</tr>

</table>

In this exercise you are an FME user at the City of Interopolis. The city is planning its maintenance budget for the year and wishes to know how many pieces of public art are erected on city-owned property.

So, your task is to use the data available to identify city properties and find out what pieces of artwork are located on them.

**1) Inspect Data**
<br>Start the FME Data Inspector and inspect the three datasets we will be using:

<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">PostGIS</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">PostGIS Training Database</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Parameters</td>
<td style="">Table List: ParcelPolygons</td>
</tr>

</table>


<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Microsoft Excel</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\Culture\PublicArt.xlsx</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Parameters</td>
<td style="">Coordinate System: LL83</td>
</tr>

</table>


<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Esri Shape</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\Parcels\CityProperties\CityProperties.shp</td>
</tr>

</table>

If you haven't realized it yet, this is going to be a tough assignment. The city owned properties are point features, not polygons, so we are going to have to use the parcel data to discover the actual area of each property. Then we will have to overlay the public art features, which at the moment are in a non-spatial format of data.

***NB:** If you don't already have the "PostGIS Training Database" connection, then look up the exercise for Chapter 3 (Best Practice) and carry out the first step in there.*

*Alternatively, if you have any problems using the PostGIS database - for example
connectivity problems with a firewall - then the following AutoCAD dataset can be
substituted with very few changes required.*

<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Autodesk AutoCAD DWG/DXF</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\Parcels\Parcels.dwg</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Parameters</td>
<td style="">GroupEntities By: Attribute Schema</td>
</tr>

</table>


**2) Start Workbench**
<br>Start Workbench and begin with an empty workspace. Because there are several formats of source data it's going to be easier to add Readers individually, rather than generate a workspace.

Choose Readers > Add Reader from the menubar. When prompted enter the format and
dataset of the city properties (Shape) dataset we are using. The dialog should be very similar to the one in the Data Inspector.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.71.AddReaderDialogShapeFormat.jpg)

Click OK to close the dialog and add the Reader to the workspace.


**3) Add Reader**
<br>Now we need to add the property boundaries (PostGIS) dataset to the workspace. Use Readers > Add Reader as in the previous step and the Reader Format/Dataset shown in step 1.

The workspace will now look like this:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.72.WorkspaceWithPostGISandShapeFeatureTypes.jpg)

***NB:** If you are unable to use the PostGIS database, this is where you can substitute the DWG dataset.*

Take a look at the parameters for this Reader in the Navigator window. Notice how there are many options for how we read individual features of the dataset, plus several others relating to database performance:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.73.PostGISReaderParameters.jpg)


**3b) Add AreaBuilder**
<br>If you are using the AutoCAD (not PostGIS) data, you may have noticed that the property boundaries were just lines, and not polygons. To turn them into polygons requires a transformation.

Add an AreaBuilder transformer and connect it to the ParcelLines feature type:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.74.AreaBuilderTransformerInWorkspace.jpg)

You can check the parameters, but none should need changing.


**4) Add PointOnAreaOverlayer**
<br>Add a PointOnAreaOverlayer transformer. This will tag the property polygons that belong to the city by carrying out an overlay with the City-Owned dataset.

Connect the CityProperties feature type to the Point input port. Connect the ParcelPolygons feature type (or AreaBuilder:Area port) to the Area input port:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.75.PointOnAreaOverlayerTransformerInWorkspace.jpg)

Again, you can check the parameters, but none should need changing. Note that the number of overlaps is denoted by an output attribute called _overlaps.


**5) Add Tester**
<br>Add a Tester transformer. It should be connected to the PointOnAreaOverlayer:Area output port. We'll use this to check the value of _overlaps. If the value is greater than or equal to 1 then we know this is a city-owned property.

Open the parameters dialog and set up a test for where _overlaps >= 1.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.76.TesterTransformerParametersDialog.jpg)

Click OK to close the dialog. You can now add some Inspector transformers to the Tester output and run the workspace to ensure that the results are correct so far. What we should get is a set of property outlines divided into city properties (Passed) and private properties (Failed).


**6) Add Reader**
<br>Now we must add a Reader to read the public art data. Choose Readers > Add Reader from the menubar. Enter the following info but don't click OK yet:


<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Microsoft Excel</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\Culture\PublicArt.xlsx</td>
</tr>

</table>


Click the Parameters button. Notice that there are a number of options to preview the data for each sheet. At the foot of the dialog is the attribute schema. 

Notice how there are attributes for Longitude and Latitude. Because the columns are actually named Longitude and Latitude, FME automatically realized that these can be used to convert this tabular data to true spatial features.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.77.ExcelReaderParametersDialog.jpg)

If the names were not as obvious, we could tell FME to automatically convert these to proper spatial features by clicking the drop-down arrow and manually selecting "x_coordinate" and "y_ coordinate":

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.78.ExcelReaderParametersDialog.jpg)

Click OK and OK again to close the dialogs and add the Reader to the workspace. Notice that there are several features types (one for each sheet in the Excel spreadsheet):

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.79.WorkspaceWithExcelFeatureTypesHighlighted.jpg)


**7) Add Reprojector**
<br>One issue to still overcome is that the data from the Excel Reader will be in Latitude/Longitude (because that's what the coordinates were saved as) whereas the other datasets are UTM83-10. We'll solve this by reprojecting the data.

Add a Reprojector transformer connected to all of the Excel feature types. Open the parameters dialog and set it to convert from LL83 to UTM83-10:


**8) Add PointOnAreaOverlayer**
<br>Now we just need to determine which of the public art features fall inside one of the city properties. We can do this with another PointOnAreaOverlayer transformer. Add one of these and connect the art features to the Points input port and the Tester:Passed features to the Area port:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.80.GeneralWorkspaceImage.jpg)


**9) Add Tester**
<br>A final Tester will help us filter the art features we need. Add a Tester connected to this new PointOnAreaOverlayer Point output port. Again set it up to test where _overlaps is >= 1.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.81.TesterInWorkspaceAfterPointOnAreaOverlayer.jpg)


**10) Run Workspace**
<br>Add an Inspector transformer to the Tester:Passed port. Save and run the workspace. The output from this will be all public art installations that reside on city property.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.82.DataInspectorDataView.jpg)

**Congratulations!** 
You have now:

- Added Readers to a workspace
- Checked the Reader parameters

