<table style="border-spacing: 0px">
<tr>
<th style="background-color:darkorange;color:white">Exercise 4c</th>
<th style="background-color:darkorange;color:white">Fixing Unexpected Input</th>
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
<td style="border: 1px solid darkorange">Handling unexpected input</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">Start Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4c-Begin.fmw</td>
</tr>

<tr>
<td style="border: 1px solid darkorange; font-weight: bold">End Workspace</td>
<td style="border: 1px solid darkorange">C:\FMEData2015\Workspaces\DesktopBasic\Exercise4c-Complete.fmw</td>
</tr>

</table>

This example fixes, in a simple way, the problem of reading multiple files in a workspace.


**1) Start Workbench**
<br>Start Workbench and open the workspace from the previous example (if it is not already open).

Remember, when this is run the Unexpected Input Remover pops up. That's because you are trying to read a layer/file called L2, when no L2 feature type exists in the workspace.

We could resolve this by using the Import Feature Type tool, but to handle all of the files in this way would result in a translation that looked like this:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.87.LotsOfFeatureTypesInAWorkspace.jpg)

So, we'll fix this translation a better way, by unlocking the single feature type we have to let any of these files past.


**2) Set Merge Filter**
<br>Open the properties dialog for the Feature Type. In the middle of the dialog select the option Merge Feature Type:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.88.MergeFilterParametersDialog.jpg)

In the Merge Filter field we can define which files we will allow to pass. Because there isn't really a common theme to the file names (or not one that we can easily define without using a regular expression) simply enter an asterisk in that field:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.89.MergeFilterParametersDialog.jpg)

This means we'll allow any and all files to pass. In fact, when you press OK, the feature type object is updated to reflect that fact:

![](https://raw.githubusercontent.com/FMEEvangelist/FME-Desktop-Basic-Training-Manual-Images/master/Img4.90.MergeFilterAsSeenOnFeatureTypeObject.jpg)


**3) Run Workspace**
<br>Now when the workspace is run the chosen data will be allowed to pass. In fact, you should be able to select any - or several - of the contour Shape files and they will all pass.

**Congratulations!** 

You have now:

- Used a Merge Filter to avoid Unexpected Input


***Doctor Workbench says...***

>"You can probably see one drawback of this method: all of the features are merged into a single bloodstream – sorry, data stream – in the workspace. If I want unique output feature types I must separate the data again.
> 
> In this case it’s not too much of a problem, as the data is all the same type just tiled separately. But you may want to reflect on when the Import Feature Type tool is a better option."