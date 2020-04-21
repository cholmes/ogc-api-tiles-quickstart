# Quickstart Guide to [OGC API - Tiles](https://github.com/opengeospatial/OGC-API-Tiles)

## About

This guide is intended to communicate the key pieces of the Tiles API standard so that implementors can get started 
without having to dig through the longer specification.

## Introduction

OGC API - Tiles provides a standard way to describe web map tile sets for clients to consume. It is the successor to 
[WMTS](https://en.wikipedia.org/wiki/Web_Map_Tile_Service), updated to use JSON, in line with the spatial data on the web 
best practices. It also is designed to be a 'building block', that can plug into the OGC API suite, but can also be used
as a standalone piece. It's also similar to [TileJSON](https://github.com/mapbox/tilejson-spec), adding the ability to 
handle alternate projections and fitting into the OGC API suite of standards.


## Tiles Description Link

Any resource that is represented in JSON can provide a link to a ‘Tiles Description’ JSON object that provides the necessary 
metadata to enable a client application to formulate a tile request. This could be an OGC Collection, OGC Item (from OGC API - 
Features), or a custom resource (even an ‘ephemeral’ resource like a search response). But the resource must have a links 
array that includes an object with a rel  of ‘tiles’ and a type of ‘application/json’, and the `href` must return the Tiles 
Description object specified below.    

```
  "links": [
    ...
    {
      "href": "http://data.example.com/collections/buildings/tiles",
      "rel": "tiles",
      "type": "application/json",
    }
  ]
}
```

This communicates to any client that the link is the tiles representation of this particular resource. 
The core specification [shows a link in a buildings Collection](https://github.com/opengeospatial/OGC-API-Tiles/blob/master/core/standard/clause_7_tile_core.adoc#tiles-description-link), which will be the starting point for many.
(TODO: possibly add more examples, perhaps at the bottom or in an /examples/ directory)

## Tile Description Response

The response of this link type is the JSON object that contains the information to enable a client application to formulate a 
tile request that represents the resources that created the link. The current minimum response in the specification is:

```
{
  "tileMatrixSetLinks": [
    {
      "tileMatrixSet": "WorldMercatorWGS84Quad",
      "tileMatrixSetURI": "http://schemas.opengis.net/tms/1.0/json/examples/WorldMercatorWGS84Quad.json"
    }
  ],
  ...
  "links": [
    ...
    {
     "href": "http://data.example.com/collections/buildings/tiles/{tileMatrixSetId}/{tileMatrix}/{tileRow}/{tileCol}.png",
     "templated": true,
     "rel": "item",
     "type": "image/png",
   }
   ...
  ]
}
```

The specification provides advanced options in the `tileMatrixSetLinks` to specify your own projections and matrices. For
now just use the exact example above if your data is in WebMercator XYZ (as most web tiles are).

TODO: Add info on how to make the template match your server. And clarify if people have to include the tileMatrixSetID if 
they just have one

