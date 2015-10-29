<table style="border-spacing: 0px">
<tr>
<th style="background-color:darkorange;color:white">Exercise 4b</th>
<th style="background-color:darkorange;color:white">Unexpected Input</th>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Scenario</td>
<td style="border: 1px solid darkorange">FME Workspace Author</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Data</td>
<td style="border: 1px solid darkorange">Contours (Esri Shapefile)</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Overall Goal</td>
<td style="border: 1px solid darkorange">Read a set of contour shapefiles</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Demonstrates</td>
<td style="border: 1px solid darkorange">Unexpected Input</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Start Workspace</td>
<td style="border: 1px solid darkorange">None</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">End Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4b-Complete.fmw</td>
</tr>

</table>

This example merely reads a file of contour data, but demonstrates a potential problem when changing the source dataset parameter.


**1) Inspect Data**
<br>Start the FME Data Inspector and open the datasets:

<table style="border: 0px">

<tr>
<td style="font-weight: bold">Reader Format</td>
<td style="">Esri Shapefile</td>
</tr>

<tr>
<td style="font-weight: bold">Reader Dataset</td>
<td style="">C:\FMEData2015\Data\ElevationModel\Contours\*.shp</td>
</tr>

</table>

In other words, select all of the Shape files in that folder.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.83.DataInspectorDataViewShapeContours.jpg)

You'll see that the contour data - because of its size - has been split up into a grid of files.


**2) Create Workspace**
<br>Start FME Workbench and begin with an empty workspace. Select Readers > Add Reader from the menubar.

When prompted enter the format and dataset for ONE of the contour files, for example O9.shp.

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.84.ShapeFeatureTypeInWorkspace.jpg)

Add an Inspector and run the workspace to ensure it works as expected.


**3) Change Dataset**
<br>Now we'll assume that someone else has asked you to translate a different file, for example L2.shp.

So, locate the source dataset parameter in the Navigator window:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.85.ShapeReaderSourceDatasetParameter.jpg)

Double-click the parameter and a Source Shape File dialog opens up. Use the browse button to select L2.shp


**4) Run Workspace**
<br>Run the workspace. You will get a dialog pop up reporting that there was "Unexpected Input":

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.86.UnexpectedInputPopupDialog.jpg)

What this means is that you've tried to read a layer of data that is not defined in the workspace.

**Congratulations!**

You have now:

- Modified the Reader dataset parameter
- Discovered a potential problem of modifying the Reader dataset parameter!