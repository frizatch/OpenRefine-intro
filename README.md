# Data Wrangling with OpenRefine!
<img src="images/OpenRefine_logo_color.png" width="200"/>

OpenRefine is a free, open-source Java application that helps you easily prep data for analysis.


## Workshop Resources
 - Software - access via the [OpenRefine's homepage](https://openrefine.org/)
 - Documentation - [OpenRefine's User Manual](https://openrefine.org/docs)
 - Data - download NBI data from this dropbox [OpenRefine workshop data](https://www.dropbox.com/s/b6h9gonvii74k4r/NBI-NationalBridgesIndex-subset.csv?dl=0)
 - Data Dictionary - [field descriptions](https://nationalbridges.com/nbiDesc.html) for the NBI data

## Workshop Goals
By the end of this workshop, you will be able to:
- Use OpenRefine to explore your data
- Use OpenRefine to accelerate the data cleaning process
- Understand the concept of GREL expressions for data transformations
- Look up external data via URLs and enrich your data

## Outline
- [Why do we need clean data?](#why-clean-data)
- What is OpenRefine
- Getting Set Up and Running OpenRefine
- Facets, Filters and Clustering
- Transformations
- Adding data using APIs

## <a name="why-clean-data"></a>  Why do we need clean data?
Why do we need clean data?

## OpenRefine

OpenRefine is a free, open-source Java application.

## Getting Set Up

### Downloading the software

https://openrefine.org/download.html

Compatible on Windows, Mac and Linux!
The mac package comes with java built in.
The windows packages have an option of with java built in or not.
NOTE: I had my java at the most recent update via my java control panel, but the windows sans java via the github assests page sent me to the Installation documention even so. Windows package with java worked fine.

### Running OpenRefine

When working in OpenRefine, you'll be interacting with a browser window. Initially a command line window will open on your machine (a window with a black background). As OpenRefine runs, lines of text will appear in the command line window, then the OpenRefine interface will open in your default web browser. Leave the command line window open in the background and let it do its thing, and work on datasets in your web browser.

### Exiting OpenRefine

To exit OpenRefine, close all the browser tabs or windows, then navigate to the command line window. To close this window and ensure OpenRefine exits properly, hold down [control] and press [c] on your keyboard. This will save all changes to your projects.

### General operation concept

OpenRefine keeps all of the data on your local machine! No internet connection is needed, and none of the data or commands you enter in OpenRefine are sent to a remote server. This does mean that your project is tied to that local machine, so export/import is necessary if you want to work on a project elsewhere.
You are NOT modifying original/raw data.
Projects are autosaved every five minutes and when OpenRefine is properly shut down (Ctrl+C).

### Loading Data

## Working with the Data

### Facets and Filters

A ‘Facet’ groups all the values that appear in a column, and then allows you to filter the data by these values and edit values across many records at the same time.

When you filter data, you're picking a certain subset of the data to work with.

Do the following and see how quickly you can subset the bridge data to those that go over "creeks":
- Go to the "features_desc_006a" column
- Choose Text Filter
- In the interactive box that appears on the left, type in "creek"
- Note the active records have gone from 250 to 110. Also note you can invert your filter.

### Clustering

## Transformations with GREL

For data manipulation, Open Refine uses GREL (General Refine Expression Language).

https://openrefine.org/docs/manual/grel

The basic idea is the data in a cell in the column you work with is represented by the "value" variable, and you can pass it through expressions to adjust the data.

These expressions allow us to do creative things. For example, we know (since we looked at our data dictionary!) that there are two columns in our NBI data that have a code indicating who maintains the bridge, and who owns the bridge. If the codes don't match, then we know there might be an added layer of complexity in maintenance operations since the owner organization isn't directly responsible for the upkeep.

We can quickly flag this information by:
- converting these columns to numbers: Edit cells -> Common Transformations -> To Number
- creating a new column based on the "maintenance_21" column using the expression: value-cells["owner_022"].value (we are subtracting the numbers)
- any non-zero value indicates the maintenance is not done by the owner!

## Undo / Redo

As mentioned before, OpenRefine is not actually changing your original data. It is writing a series of steps that it uses to transform the data as it appears on your local server. You can go to the Undo/Redo tab and back up to a previous step. Only when you create a different fork of transformation, do you lose the steps that were downline.

To keep the transformed data, you need to use the Export option.

## External Data - using APIs

Good datasets have a column of unique identifiers that you can hopefully use to attach more information you may want. Think of ISSN numbers for publications or FIPS codes for geographic areas. In our NBI dataset, we have column called ObjectId that we can use to link to more information via the National Bridges Inventory API.

(okay, I'm coming clean: for this exercise, I provided a subset of the over 8,000 bridges in CO, and a subset of the columns that you can see in the data dictionary. What we're going to do now is use the API to go back and snag a useful column I cut off earlier. But this is to demonstrate how the "Add column by fetching URLs..." option works!)

In our assessment of bridges, one useful data point could be the year the bridge was built (Item 27 in the data dictionary). Let's retrieve it from the online NBI database...

Examine this ridiculous [link](https://geo.dot.gov/server/rest/services/Hosted/National_Bridge_Inventory_DS/FeatureServer/0/query?where=&objectIds=65059&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=year_built_027&returnGeometry=true&maxAllowableOffset=&geometryPrecision=&outSR=&havingClause=&gdbVersion=&historicMoment=&returnDistinctValues=false&returnIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&multipatchOption=xyFootprint&resultOffset=&resultRecordCount=&returnTrueCurves=false&returnCentroid=false&timeReferenceUnknownClient=false&sqlFormat=none&resultType=&datumTransformation=&lodType=geohash&lod=&lodSR=&f=pjson) (See below). It is a URL that is hitting a json return of the API for retrieving information from the NBI database for a particular bridge and it's year-built attribute. To turn it into a useful GREL expression for our purposes, we'll adjust the objectId to equal our value, as opposed to this one bridge that has the objectId 65059. The following text is adjusted and you can copy & paste it, but understand that you're pointing to the json at the URL with the objectId inserted:
```
https://geo.dot.gov/server/rest/services/Hosted/National_Bridge_Inventory_DS/FeatureServer/0/query?where=&objectIds=" + value + "&time=&geometry=&geometryType=esriGeometryEnvelope&inSR=&spatialRel=esriSpatialRelIntersects&distance=&units=esriSRUnit_Foot&relationParam=&outFields=year_built_027&returnGeometry=true&maxAllowableOffset=&geometryPrecision=&outSR=&havingClause=&gdbVersion=&historicMoment=&returnDistinctValues=false&returnIdsOnly=false&returnCountOnly=false&returnExtentOnly=false&orderByFields=&groupByFieldsForStatistics=&outStatistics=&returnZ=false&returnM=false&multipatchOption=xyFootprint&resultOffset=&resultRecordCount=&returnTrueCurves=false&returnCentroid=false&timeReferenceUnknownClient=false&sqlFormat=none&resultType=&datumTransformation=&lodType=geohash&lod=&lodSR=&f=pjson
```

