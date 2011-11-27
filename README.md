# Translink

Scraper for http://translink.com.au/. Very much a work-in-progress. Use with caution.

## Usage

Import all routes and services for Thursday, 24 November 2011.

    $ translink import 2011-11-24
    
Optionally specify the URI to the database.

    $ translink import 2011-11-24 --uri="postgres://user:secret@127.0.0.1/translink"
    $ translink import 2011-11-24 --uri="sqlite:///Users/Tate/Downloads/translink.sqlite3"

## Copyright

Copyright © 2011 Tate Johnson. Released under the MIT license. See LICENSE.
