# mu-javascript-template

Template for writing mu.semte.ch services in JavaScript

## Getting started

Create a new folder.  Add the following Dockerfile:

    FROM semtech/mu-javascript-template
    MAINTAINER Aad Versteden <madnificent@gmail.com>

Create your microservice:

    import { app, query } from 'mu';
    
    app.get('/', function( req, res ) {
      res.send('Hello mu-javascript-template');
    } );


    app.get('/query', function( req, res ) {
      var myQuery = `
        SELECT *
        WHERE {
          GRAPH <http://mu.semte.ch/application> {
            ?s ?p ?o.
          }
        }`;

      query( myQuery )
        .then( function(response) {
          res.send( JSON.stringify( response ) );
        })
        .catch( function(err) {
          res.send( "Oops something went wrong: " + JSON.stringify( err ) );
        });
    } );
    
## Requirements

  - **database link**: You need a link to the `database` which exposes a sparql endpoint on `http://database:8890/sparql`.  In line with other microservices.

## Imports

The following importable variables are available:

  - `app`: The application on which routes can be added
  - `query(query) => Promise`: Function for sending queries to the triplestore
  - `update(query) => Promise`: Function for sending updates to the triplestore

You can either import specific attributes from the mu library, or import the whole mu object.

An example of importing specific variables:

    import { app, query } from 'mu';
    
    app.get('/', function( req, res ) {
      res.send('Hello mu-javascript-template');
    } );

An example of importing the whole library:

    import mu from 'mu';
    
    mu.app.get('/', function( req, res ) {
      res.send('Hello using full import');
    } );

## Developing with the template

When developing, you can use the template image, mount the volume with your sources on `/app` and add a link to the database.

    dr run --link virtuoso:database \
           -v `pwd`:/app \
           -p 8888:80 \
           --name my-js-test \
           semtech/mu-javascript-template


