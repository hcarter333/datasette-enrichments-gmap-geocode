# datasette-enrichments-gmap-geocode

[![PyPI](https://img.shields.io/pypi/v/datasette-enrichments-gmap-geocode.svg)](https://pypi.org/project/datasette-enrichments-gmap-geocode/)
[![Changelog](https://img.shields.io/github/v/release/hcarter333/datasette-enrichments-gmap-geocode?include_prereleases&label=changelog)](https://github.com/hcarter333/datasette-enrichments-gmap-geocode/releases)
[![Tests](https://github.com/hcarter333/datasette-enrichments-gmap-geocode/workflows/Test/badge.svg)](https://github.com/hcarter333/datasette-enrichments-gmap-geocode/actions?query=workflow%3ATest)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com/hcarter333/enrichments-gmap-geocode/blob/main/LICENSE)

Geocoding enrichment using Google Maps API
Everything in this repository is a refinement of the [OpenCage](https://datasette.io/plugins/datasette-enrichments-opencage) repository adapted to use Google Maps instead
## Installation

Install this plugin in the same environment as Datasette.
```bash
datasette install datasette-enrichments-gmap-geocode
```
## Usage

This plugin adds an enrichment for geocoding using the [Google Maps Geocoder](https://developers.google.com/maps/documentation/geocoding/overview).

You will need an API key from Google Maps - you can sign up for a key that includes free usage that's fairly large [Google Maps API Key](https://developers.google.com/maps/documentation/geocoding/get-api-key).

Filter for data that you wish to geocode, then apply the Google Maps API geocoder enrichment.

You'll need to specify a template to be passed to the geocoder, specifying which templates should be used as the input.

If you have a single column containing the address, you can use this:

    {{ address }}

If you have separate columns for the street, city, state and country, you can use this:

    {{ street }}, {{ city }}, {{ state }}, {{ country }}

If your address column is missing the country, but all of the addresses are in the USA, you could use this:

    {{ address }}, USA

See the Googlel Maps Geocoding [docs](https://developers.google.com/maps/documentation/geocoding/requests-geocoding) for tips on how to get the best results.

By default only the latitude and longitude from the geocoder will be stored, in the `latitude` and `longitude` columns on your table. These columns will be created if they do not yet exist.

You can optionally specify a column to store the full JSON output of the geocoder. This column will also be created if it does not exist.

The full JSON format is [described here](https://developers.google.com/maps/documentation/geocoding/requests-geocoding#json).

## Configuration

You can use this plugin without configuration, but you'll need to enter your API key every time you run an enrichment.

To avoid that, you can set your API key as plugin configuration like this:

```bash

export MAPS_API_KEY="your-api-key"
```
Then in `metadata.yml`:
```yaml
plugins:
  datasette-enrichments-gmap-geocode:
    api_key:
      $env: MAPS_API_KEY
```
Then run Datasette like this:
```bash
datasette mydatabase.db -m metadata.yml --root
```
This well give you a URL to sign in as the "root" user, which grants you access to the enrichment.

## Development

To set up this plugin locally, first checkout the code. Then create a new virtual environment:
```bash
cd datasette-enrichments-gmap-geocode
python3 -m venv venv
source venv/bin/activate
```
Now install the dependencies and test dependencies:
```bash
pip install -e '.[test]'
```
To run the tests:
```bash
pytest
```
Command for testing with freedraw  
```bash
python3 -m datasette gm_test.db --metadata qso_loc.yml --load-extension=/usr/lib/x86_64-linux-gnu/mod_spatialite.so --template-dir plugins/templates --root
```
