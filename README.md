# mta-bus-geojson
A python module to build (or refresh) detailed and up-to-date geojson files for each of the MTA's bus routes in NYC.

## details
The MTA provides [a series of text files](http://web.mta.info/developers/developer-data-terms.html) that contain schedules, route shapes (sets of coordinates), route metadata, and stop information. Somewhat confusingly, every bus route has several slightly different shape variations, depending on time of day and bus direction. This module selects the **most common** route shape in each route direction (0 or 1) from the current MTA schedule, and outputs geojson with filename:

*[route_id]_[direction].geojson (eg. Bx39_0.geojson)*

Bus stops are represented as Point features, and the line segments connecting the stops are LineString features. Metadata is provided for all features under a *properties* key:
- stop_id
- stop_name
- stop_sequence

Where stop_id is the MTA's integer id for the stop, stop_name is a user-friendly address string, and stop_sequence is an integer that represents the order of stops throughout the route. The LineString feature that is directly **after** a given bus stop along a route will share the same metadata as that stop.

Note that any existing geojson files in the target directory are deleted when the script is run.

## usage

`python rebuild_geojson.py <target_directory>`

If <target_directory> not provided, files will be written to current directory.
