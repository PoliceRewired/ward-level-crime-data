# ward-level-crime-data
Tools for retrieving ONS ward boundaries and obtaining monthly data.police.uk street-level crimes per ward.

# Generating crime data per ward
The goal is to obtain a monthly set of crime data per voting ward. To do so, we will combine ONS ward data with the Police UK data API. The rationale for this is explained below.

## Sources
The Police UK data [street-level crime API](https://data.police.uk/docs/method/crime-street/) provides crime data by point (1km around a lat/lng), or by polygon (specify the boundary and retrieve the crimes recorded within that shape).

It also provides the shapes of individual [police neighbourhoods](https://data.police.uk/docs/method/neighbourhood-boundary/). However, these do not tie up so neatly with voting wards, and so we need another source of boundary lines.

ONS produce four sets of ward outline data: full resolution ward boundaries, generalised ward boundaries, [super generalised ward boundaries](https://geoportal1-ons.opendata.arcgis.com/datasets/d5c9c1d89a5a44e9a7f88f182ffe5ba2_3), and ultra generalised ward boundaries. Each provides the boundaries of the wards across the UK in different levels of detail. For our purposes, we will be using super generalised boundaries - they’re good enough for what we’re looking at and provide simple enough polygons to include in the GET for the police data API. (It’s possible to POST more complex polygons, but seems unnecessary.)

## Method
The ward boundaries are available as shape files and also as KML. We will be parsing the KML to extract ward name, area name, a unique id, and the boundary polyline.

Once we have this data, we need only alter the boundary polyline format a little to generate CURL commands that can fetch the crime data for each ward.

## Parsing the wards KML
We can parse the KML using a Google Drive apps-script. There are approximately 7800 wards in the UK, and apps-scripts are limited to 5-6 mins of runtime - so this work is broken up into several runs.

### Steps
* Download and store the [ward boundaries KML](https://geoportal1-ons.opendata.arcgis.com/datasets/d5c9c1d89a5a44e9a7f88f182ffe5ba2_3) in your drive.
* Copy and paste this apps-script into your drive: [ward_KML_parser.js](https://gist.github.com/lewis-policecoder/17059cea37c29f189764db981165c5b7)
* Modify the run_partial() function to work on ~1500 entries max. Run the function.
* Repeat, modifying the script and running again until you have parsed all the wards.
* You can view this ward data in the freshly created sheet: parsed-sheet

## Generating the CURL requests
We can process the parsed-sheet sheet generated above to generate a sequence of CURL commands that can fetch the crime data per ward.

### Transformations
* The boundary is provided as: lat,lng lat,lng lat,lng
* It is required as: lat,lng:lat,lng:lat,lng
* The CURL commands will also provide an output filename with the ward’s unique id.
* Spaces and apostrophes will need to be removed from the filename.

### Steps
* Copy/paste this script into the sheet: [street_level_crime_CURL_generator_per_ward.js](https://gist.github.com/lewis-policecoder/288a3c35295b59f0d243d57433cc6619)
* Run the script. You will be prompted for the range of rows to work on.
* Provide a max of 1000 rows.

The CURL commands will be generated and added to column E in the sheet.

_At current time, these commands will fetch the most recent month’s worth of data - but this can be easily amended by appending &date=YYYY-MM to the querystring._

