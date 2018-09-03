# Voting ward crime data

Voting ward boundaries are slightly different from policing ward boundaries, for pragmatic reasons.

This process allows us to generate boundaries we can pass to data.police.uk to collect a cut of crime data per ward per month.

## Just here for the data?

Take a look inside the __2018-05 boundaries__ folder - you'll find subfolders for the data that has been retrieved.

* __london_month_YYYY_MM__ - a folder for London data. JSON files contain crime data for each London ward in the given month.
* __UK_month_YYYY_MM__ - a folder for UK data. JSON files contain crime data for every UK ward in the given month.

## Repository structure

* __/READ.md__ - this README.
* __/.gitignore__ - mainly to prevent accidental inclusion of large KML files in this repository.
* __/ward identities/Wards - wards.csv__ - comma separated ward identity data (id, ward name, ward area)
* __/ward identities/Wards - wards.tsv__ - tab separated ward identity data (id, ward name, ward area)
* __/2018-05 boundaries/london_ward_ids_only.csv__ - a list of ids (one per line) for the wards in London (ostensibly CSV).
* __/2018-05 boundaries/ward boundary processing.workbook__ - fetches crime data for the whole of the UK for a given month.
* __/2018-05 boundaries/london wards processing.workbook__ - fetches crime data for all London wards across a range of months.

## Fetching ward boundaries

Boundaries were updated (provisionally) in May 2018, so we'll use the latest boundaries from ONS.

* [May 2018 boundaries from ONS](https://geoportal1-ons.opendata.arcgis.com/datasets/fba403b550d3456b813714bfbe7d0f0c_0) (~500Mb)

Download and place the KML file for these boundaries in the __2018-05 boundaries__ folder.

## Retrieving crime data per ward

You will need to install:

* [Xamarin Workbooks](https://developer.xamarin.com/workbooks/)

The Xamarin Workbooks inside the __2018-05 boundaries__ folder manage retrieval of crime data.

To fetch your own subset of the data, open the appropriate workbook, update the initial variables, and use Ctrl+R to run.

__Please note, at time of writing there are 9114 wards across the UK with ~13000 boundaries defined (as some wards have multiple boundaries). The process of extracting crime data for a given month can take a considerable amount of time!__

### Notes on ward boundaries

Our ward boundaries are wrapped in KML and not in the format we'll need to generate requests to the data.police.uk APIs.

First we'll extract the boundary data per ward, then we'll use that to POST a request to data.police.uk and retrive a JSON file
with details of all crime in a given month for that ward.

__NB:__

* The data.police.uk API cannot handle complex ward boundaries by GET, but you may submit a full boundary in POST.
* The __poly__ parameter for the API is counter-intuitively arranged: __LNG:LAT,LNG:LAT,LNG:LAT,etc.__
* The __month__ parameter is formatted: YYYY-MM (as you might expect)

## Understanding crime data

Please see the __street-level__ crime endpoint page at data.police.uk for the range of categories and information you can retrieve:

* https://data.police.uk/docs/method/crime-street/
