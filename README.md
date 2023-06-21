# Cleaning data with OpenRefine!
<img src="images/OpenRefine_logo_color.png" width="200"/>

OpenRefine is a free, open-source Java application that helps you easily prep data for analysis.


## Workshop Resources
 - Software - access via the [OpenRefine's homepage](https://openrefine.org/)
 - Documentation - [OpenRefine's User Manual](https://openrefine.org/docs)
 - Data - download from this dropbox [OpenRefine workshop data folder](https://www.dropbox.com/___________)
 - Data Dictionary - [field descriptions](https://nationalbridges.com/nbiDesc.html) for the NBI data

## Workshop Goals
By the end of this workshop, you will be able to:
- Use OpenRefine to accelerate the data cleaning process

## Outline
- [Why do we need clean data?](#why-clean-data)
- 

## <a name="why-clean-data"></a>  Why do we need clean data?
Why do we need clean data?

## OpenRefine

OpenRefine is a free, open-source Java application.

### Downloading the software

https://openrefine.org/download.html

Compatible on Windows, Mac and Linux!
The mac package comes with java built in.
The windows packages have an option of with java built in or not.
NOTE: I had my java at the most recent update via my java control panel, but the windows sans java via the github assests page sent me to the Installation documention even so. Windows package with java worked fine.

### Running OpenRefine

When running OpenRefine, initially a command line window will open. This is a window with a black background. As OpenRefine runs, lines of text will appear in the command line window. Then the Open Refine interface will open in your default web browser. You do not need to interact with the command line window. Leave it open in the background, and work on datasets in your web browser.

### Exiting OpenRefine

To exit OpenRefine, close all the browser tabs or windows, then navigate to the command line window. To close this window and ensure OpenRefine exits properly, hold down [control] and press [c] on your keyboard. This will save all changes to your projects.

### GENERAL

No internet connection is needed, and none of the data or commands you enter in OpenRefine are sent to a remote server.
You are NOT modifying original/raw data.
Projects are autosaved every five minutes and when OpenRefine is properly shut down (Ctrl+C). See History in User Manual for details.
Files are saved locally such that if you are working on two computers you will have to export/import files/projects.

#### Facets

A ‘Facet’ groups all the values that appear in a column, and then allows you to filter the data by these values and edit values across many records at the same time.

#### Filters

When you filter data, you're picking a certain subset of the data to work with.

## GREL
For data manipulation, Open Refine uses GREL (General Refine Expression Language).

https://openrefine.org/docs/manual/grel


## External Data - using APIs

Add column by fetching URLs!!!

Examine this ridiculous [link](https://geo.dot.gov/server/rest/services/Hosted/National_Bridge_Inventory_DS/FeatureServer/0/query?where=&objectIds=65059&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=year_built_027&returnGeometry=true&maxAllowableOffset=&geometryPrecision=&outSR=&havingClause=&gdbVersion=&historicMoment=&returnDistinctValues=false&returnIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&multipatchOption=xyFootprint&resultOffset=&resultRecordCount=&returnTrueCurves=false&returnCentroid=false&timeReferenceUnknownClient=false&sqlFormat=none&resultType=&datumTransformation=&lodType=geohash&lod=&lodSR=&f=pjson) (See below). It is a URL that is hitting a json return of the API for retrieving information from the NBI database for a particular bridge and it's year-built attribute. To turn it into a useful GREL expression for our purposes, we'll adjust the objectId to equal our value, as opposed to this one bridge that has the objectId 65059. The following text is adjusted and you can copy & paste it, but understand that you're pointing to the json at the URL with the objectId inserted:
```
https://geo.dot.gov/server/rest/services/Hosted/National_Bridge_Inventory_DS/FeatureServer/0/query?where=&objectIds=" + value + "&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=year_built_027&returnGeometry=true&maxAllowableOffset=&geometryPrecision=&outSR=&havingClause=&gdbVersion=&historicMoment=&returnDistinctValues=false&returnIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&multipatchOption=xyFootprint&resultOffset=&resultRecordCount=&returnTrueCurves=false&returnCentroid=false&timeReferenceUnknownClient=false&sqlFormat=none&resultType=&datumTransformation=&lodType=geohash&lod=&lodSR=&f=pjson
```
more things
