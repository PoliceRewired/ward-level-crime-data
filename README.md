# Voting ward crime data

Voting ward boundaries are slightly different from policing ward boundaries, for pragmatic reasons.

This process allows us to generate boundaries we can pass to data.police.uk to collect a cut of crime data per ward per month.

You will need to install:

* [Xamarin Workbooks](https://developer.xamarin.com/workbooks/)

## Fetch ward boundaries

Boundaries were updated (provisionally) in May 2018, so we'll use the latest boundaries from ONS.

* [May 2018 boundaries from ONS](https://geoportal1-ons.opendata.arcgis.com/datasets/fba403b550d3456b813714bfbe7d0f0c_0) (~500Mb)

Download and place the KML file for these boundaries in the same directory as __ward boundary processing.workbook__.

## Processing boundaries

These boundaries are wrapped in KML and not in the format we'll need to generate requests to the data.police.uk APIs.

First we'll extract the boundary data per ward, then we'll use that to POST a request to data.police.uk and retrive a JSON file
with details of all crime in a given month for that ward.

Please note:

* The data.police.uk API cannot handle complex ward boundaries by GET, but you may submit a full boundary in POST.
* The __poly__ parameter for the API is counter-intuitively arranged: __LNG:LAT,LNG:LAT,LNG:LAT,etc.__
* The __month__ parameter is formatted: YYYY-MM (as you might expect)

Instructions and code for doing this are all available in the Xamarin Workbook: 

* __ward boundary processing.workbook__

