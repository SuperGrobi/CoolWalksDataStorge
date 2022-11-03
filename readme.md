# Preliminary data for the CoolWalks Visualisation team

Here are files with some sample data to get you started on the visualisation side of the CoolWalks project. There is a lot of data, most of which you probably won't need.
You have:
## buildings.csv
this file contains all the building footprints used to generate the shadows. This is the csv version of the shapefile from the emu analytics dataset, with all the coordinates transformed to WSG84.
The following columns are available:
- geometry: polygon data in WellKnownText format. This should be easy enough to parse, basically a list of comma-separated tuples lon lat, lon lat, ...
- id: id from the british building footprint database
- ID: another ID. (ignore.)
- height_mean: mean height of lidar data across building footprint. Used for calculating the shaodows
- heigt_max: maximum height of lidar data across building footprint.
- height_min: useless. always 0. I should probably have deleted this colum...
- Shape_Length, Area, BNG_Area: circumference, and area of the footprint. Also not so interesting, maybe to validate parsing or something like that (be carefull about units, these should be in powers of meters...)

## shadows.csv
this file contains the shadows of all the buildings in buildings.csv, using the key height_mean as height data. The shadows represent the shadows at 2022-10-07T20:30:00 (GMT+1)
- geometry: same structure as in buildings.csv
- id: id of building the shadow belongs to.


## nodes.csv
here you have all the nodes in the simplified street network (currently, this is only the drive network, the bike/foot network will look slightly different. But it will be a network nontheless.)
- osm_id: ID of the node in OSM (0 if node is helper (used to distinguish between self and multi-edges))
- lat: latitude of the node
- lon: longitude of the node
- vertex_id: id of the node in the graph (needed for edges)
- pointgeom: Point in WKT Format. Duplicates lat and lon keywords (used in my code to transform the graph between different coordinate systems... ignore... or use... I don't care.)
- helper: whether the node is a helper node. (missing if not helper, else true...(equivalent to osm_id == 0))

## edges.csv
these are the edges of the simplified graph. Each edge has a lot of data attached to it...

- osm_id: id of OSM way associated with this edge (not unique)
- geomlength: dummy data for the length of this edge.
- src_id: id of source vertex (references vertex_id)
- dst_id: id of destination vertex (references vertex_id) (be carefull about the order. The graph is directed.)
- ~~geolinestring~~ edgegeom: linestring of the actual streets connecting the source and destination in WKT.
- ~~shadow_frac: dummy data stating which fraction of the edge is in shadow~~~
- shadowed_length: length of road [m]
- full_length: length of road in [m]
- shadowed_part_length: sum on lengths in individual shadows (should be >= shadowed_length (if this is not the case (up to numerical fluctuations in 1e-6 or something, let me know, that would be bad.))) (debugging property, I did not yet delete)
- shadowgeom: geometry of shadow, following the street line in WKT. There may be overlaps where shadows overlap. I am working on code to merge these overlaps
- helper: whether the edge is a helper edge
- tags: dictionary containing the tags of the original way. not really relevant. Ignore if you will.
- parsing_direction (direction in which I had to go through the original way to get the geometry for this edge. super-ignore please.)
- shadowpartgeom (geometry of shadowintersections before unifying. ignore please)

here again, mainly depending on whether the edge is a helper or real, some of the other entries will be missing. Parse with care.

(Running the shadow intersection code now takes about 2 to 3 minutes (as opposed to the original (3hours and 50 minutes))(on my mac book air m1))