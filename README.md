# Vrachtverkeer route > 7,5 ton Centrum Amsterdam
implementation of nlmaps and osmtogeojson for Amsterdam

This repository builds `nlmaps` with a configuration file for Amsterdam, specifying Amsterdam's map styling and map layers. In addition, this repository contains several wrapper scripts which bundle the resulting `nlmaps` build with functionality for specific use cases, like querying certain API's when the map is clicked. 

This implementation is a fork of:
- https://github.com/webmapper/amaps

And uses this javascript library:
- https://github.com/tyrasd/osmtogeojson

Which is also used in http://overpass-turbo.eu/s/Aqy

To see the live version:

https://amsterdam.github.io/truck_route_osm_amsterdam_nlmaps


see [`src/README.md`](examples/README.md) for explanation on usage. Below is documentation on the build/development setup.

## How it works
This repo installs a local copy of `nlmaps`, then compiles it with the custom configuration file at `config/amsterdam.config.js`.
In `docs/` are html and js files:

 `index.html`: is the configuration of `nlmaps` and `osmtogeojson`


Versioning
----------
Changes to the config file (`config/amsterdam.config.js`) and to the functionality of the wrapper scripts should be reflected by updating the `version` field in `package.json`.

When `nlmaps` receives an update, the `nlmaps_version` field in `package.json` should be incremented to the new nlmaps version. The build process for `amaps` looks at this field to determine which release of `nlmaps` to pull and build.


Usage summary
-------------

### with npm, for local development

1. `npm install`
2. `npm run nlmaps` fetches and installs latest release of `nlmaps` and builds with custom config
3. `npm run build-amaps` compile the wrapper scripts and assets.

To serve (required before running `test` and `lint`): `npm run serve`.

To test: `npm run test`

To lint: `npm run lint`


#### development server
Instead of running the above commands separately, you can run a live-reloading development server: `npm run dev`. This watches `src/` and the config file in `config/amsterdam.config.js`, rebuilds and runs tests on changes. You can access the demo html pages at:

    localhost:8080/index.html
    localhost:8080/mora.html
    localhost:8080/tvm.html

If you want to serve and test without using the `dev` command, the server needs to be running in a separate terminal window.

#### production build and releasing

to build for production, which puts output in `dist/` instead of `test/dist/`, run:


`npm run build-amaps -- --production`

`dist/` will contain browser and Ecmascript builds of the Javascript for the `amaps` applications.

copy these files to the docs folder and all is updated.


### with docker (used by Jenkins CI)

from a fresh install with no `build_nlmaps` docker image on your machine:

To run the http server in Docker: `docker-compose up --build -d serve` (accessible on port 8095)

To run tests and lint:

    docker-compose up --build --exit-code-from test test
    docker-compose up --build --exit-code-from lint lint
