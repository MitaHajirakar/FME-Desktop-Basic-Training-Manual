<table style="border-spacing: 0px">
<tr>
<th style="background-color:darkorange;color:white">Exercise 4d</th>
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
<td style="border: 1px solid darkorange">Managing and controlling Feature Types</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Start Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4d-Begin.fmw</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">End Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4d-Complete.fmw</td>
</tr>

</table>

This example continues where a previous one left off. In that example you created a workspace to determine what pieces of public artwork are located on city property.

Now the data must be written out so that it can be used by the finance team. However, before that, they have asked that we also divide the data up into separate postcodes.


**1) Inspect Data**
<br>Start the FME Data Inspector and open the datasets:

<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Esri Shape</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\Addresses\ForwardSortationAreas.shp</td>
</tr>

</table>

This dataset contains a set of polygons marking the postcode boundaries for the city.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.91.DataInspectorDataViewPolygons.jpg)


**2) Update Dataset Parameter**
<br>Start Workbench and open the beginning workspace. To get this data into the workspace we could simply add a new Reader. However, since we already have a Reader to handle Shape data, we can use that, update the source dataset parameter, and simply import the feature type definition.

So, firstly locate the source dataset parameter for the Shape Reader:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.92.ShapeReaderSourceDatasetParameter.jpg)

Double-click the parameter to edit it. If the new postcode Shape file was in the same folder as the CityProperties Shape file, we could simply select both of them with the regular browser.

However, since they are not, we must do this a little differently.

Click the drop-down arrow and select Open Advanced Browser.

In the advanced browser, click Add Files, browse to the postal code data
(ForwardSortationAreas.shp) and select it:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.93.AdvancedDatasetBrowserDialogShape.jpg)

Click OK and OK again to close these dialogs.


**3) Import Feature Type**
<br>Now both Shape files will be read when the workspace is run. However, the postcode data will be discarded because there is no matching feature type.

So, select Readers > Import Feature Type from the menubar.

When prompted, select the Shape Reader to import the feature type to. Then, in the next dialog, set the format and dataset parameters, the same way as they usually are.

Now click OK. When prompted to select the types to import, you can click OK again. CityProperties won't be added again because FME recognizes it exists already. However, a feature type for ForwardSortationAreas will be created:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.94.ShapeFeatureTypesInWorkspace.jpg)


**4) Add PointOnAreaOverlayer**
<br>Now add a further PointOnAreaOverlayer transformer to copy the postcode info onto matching art features. The ForwardSortationAreas will be the Areas, the output from the final Tester will be the Points:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.95.GeneralWorkspaceWithPointOnAreaOverlayer.jpg)

If you add an Inspector and run the translation now, you'll find that there is a postcode (CFSAUID) attribute added to the public art features.


**5) Add Writer**
<br>The finance team (who requested this data) would like it as a SpatiaLite database. So add a Writer with the following parameters:

<table style="border: 0px">

<tr>
<td style="font-weight: bold">Writer Format</td>
<td style="">SpatiaLite</td>
</tr>

<tr>
<td style="font-weight: bold">Writer Dataset</td>
<td style="">C:\FMEData2015\Output\Training\PublicArt.sl3</td>
</tr>

</table>


![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.96.AddWriterDialogSpatiaLite.jpg)

Add Feature Type(s)" section of the dialog, choose the option to "Copy from Reader." When prompted, select one of the existing Public Art Reader feature types- for example Mount Pleasant - and click OK. A new Writer feature type will be added that is a copy of the Reader equivalent.

Connect it to the final transformer output:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.97.SpatiaLiteWriterFeatureType.jpg)


**6) Edit Schema**
<br>The schema we created is not exactly what we want, so it should be edited. What we need is:

- name - string(46)
- title - string(32)
- address - string(30)
- postcode - string(3)

So open the properties dialog for the new Feature Type. Click on the User Attributes tab and edit the schema to what is required, but don't click OK yet. Notice that we can't use upper-case characters in a field name for SpatiaLite.


**7) Set Feature Type Parameters**
<br>Now click on the Format Parameters tab. These are parameters that apply to this table. We're only creating one table, but if there were more each could have different values for these parameters. Ensure Create Spatial Index is set to yes, so that the output dataset can be queried more efficiently:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.98.FeatureTypeDialogFormatParameters.jpg)

Again, don't click OK yet.


**8) Set "Fanout"**
<br>Click on the General tab. Check the box marked Fanout by Attribute and select the attribute CFSAUID:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.99.FeatureTypeDialogGeneralFanout.jpg)

A fanout is an advanced function that will be useful here. By checking this box, and selecting the postcode attribute, we will get a separate table of features for each different postcode.

Click OK to close the dialog.


**9) Map Schema**
<br>The final step is to map the schema correctly, as nothing is connecting up as yet. Draw connections between:

- Name - name
- Title - title
- Address - address
- CSFAUID - postcode

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.100.WorkspaceMappedSchema.jpg)


**10) Run Workspace**
<br>Save and run the workspace. Inspect the output in the FME Data Inspector. You should find there are 70 features on a number of layers, each defined by a postal code:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.101.DataInspectorDisplayControlWindow.jpg)


**Congratulations!**
<br>You have now:

- Imported a Feature Type Definition
- Added a Writer
- Added a Writer Feature Type
- Modified Feature Type parameters
- Used a fanout