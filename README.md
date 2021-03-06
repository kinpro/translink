# Deprecated

**TL;DR** This gem is deprecated and will no longer be maintained. You
should source data from the [Queensland Government](https://data.qld.gov.au/dataset/general-transit-feed-specification-gtfs-seq).

This project was created to source public transport data for South-East
Queensland. The Queensland Government has since released the data on their
own website under a liberal license. I encourage you to source your
data from an [officially supported source](https://data.qld.gov.au/dataset/general-transit-feed-specification-gtfs-seq).

# Translink

[![Build Status](https://secure.travis-ci.org/tatey/translink.png)](http://travis-ci.org/tatey/translink)

[TransLink](http://translink.com.au/) (Organisation) coordinates public transport operations in
South-East Queensland. Their website has an abundance of data with no easy way for a developer
to query it.

Translink (Program) scrapes bus routes, trips, stops and times into a relational database.
The schema is a subset of the [General Transit Feed Specification](https://developers.google.com/transit/gtfs/reference).
Data is sourced from the [TransLink website](http://translink.com.au/).You should be
aware their data is protected by [copyright](http://translink.com.au/site-information/legal/copyright).

Translink (Program) was created to provide data for my iOS app, [Next Stop](http://nextstop.me).

## Installation

Translink requires Ruby 1.9.2 or greater. For documentation on how to install Ruby on your
platform, visit the [Download Ruby](http://www.ruby-lang.org/en/downloads/) page.

Translink is available as a gem. On UNIX-like platforms, install translink from the command line.

    $ [sudo] gem install translink

## Usage

Scrape all bus stops, routes and services for Thursday, 24 November 2011 saving
them into a SQLite database named "2011-11-24.sqlite3" in the current working directory.

    $ translink scrape 2011-11-24

Change the path to the SQLite database.

    $ translink scrape 2011-11-24 ~/Downloads/translink.sqlite3

Get a specific route.

    $ translink scrape 2011-11-24 ~/Desktop/translink.sqlite3 http://jp.translink.com.au/travel-information/network-information/buses/435/2011-11-24 0"

## Queries

Stops the 130 visits on the outbound trip.

    SELECT stops.*, stop_times.*
    FROM routes
    INNER JOIN trips ON trips.route_id = routes.route_id
    INNER JOIN stop_times ON stop_times.trip_id = trips.trip_id
    INNER JOIN stops ON stops.stop_id = stop_times.stop_id
    WHERE routes.short_name = '130' AND trips.headsign = 'outbound'
    GROUP BY stops.stop_id
    ORDER BY stop_times.stop_sequence;

Routes that visit the 'Calam Rd near Honeywood St' stop.

    SELECT DISTINCT(routes.short_name) FROM stops
    INNER JOIN stop_times ON stop_times.stop_id = stops.stop_id
    INNER JOIN trips ON stop_times.trip_id = trips.trip_id
    INNER JOIN routes ON routes.route_id = trips.route_id
    WHERE stops.stop_name = 'Calam Rd near Honeywood St';

## Schema

Schema is a subset of the [General Transit Feed Specification](https://developers.google.com/transit/gtfs/reference)
defined by Google for [Google Transit](https://developers.google.com/transit/google-transit).

![Class Analysis Diagram](https://github.com/tatey/translink/raw/master/doc/schema.png)

### Deviations from the General Transit Feed Specification

* `trips.service_id` is omitted. This cannot be extracted from the dataset.
* `routes.agency_id` is omitted. There is only one agent. The agent is Brisbane Transport.

## Contributing

If you would like to help, please browse the [issues](https://github.com/tatey/translink/issues).

1. Fork it
2. Install dependencies (`bundle install`)
3. Create your feature branch (`git checkout -b my-new-feature`)
4. Run tests (`rake test`)
5. Commit your changes (`git commit -am 'Added some feature'`)
6. Push to the branch (`git push origin my-new-feature`)
7. Create new Pull Request

## Copyright

Doing something interesting with this data? Shoot me an [e-mail](mailto:tate@tatey.com). I'd love to see how
this is being used. An acknowledgement of this project is appreciated, but not required.

Copyright © 2011-2012 Tate Johnson. Released under the MIT license. See LICENSE.
