Here are files with some sample data to get you started on the visualisation side of the CoolWalks project. There is a lot of data, most of which you probably won't need.
You have:
buildings.csv
this file contains all the building footprints used to generate the shadows. This is the csv version of the shapefile from the emu analytics dataset, with all the coordinates transformed to WSG84.
The following columns are available:
- geometry: polygon data in WellKnownText format. This should be easy enough to parse, basically a list of comma-separated tuples lon lat, lon lat, ...
- id: id from the british building footprint database
- ID: another ID. (ignore.)
- height_mean: mean height of lidar data across building footprint. Used for calculating the shaodows
- heigt_max: maximum height of lidar data across building footprint.
- height_min: useless. always 0. I should probably have deleted this colum...
- Shape_Length, Area, BNG_Area: circumference, and area of the footprint. Also not so interesting, maybe to validate parsing or something like that (be carefull about units, these should be in powers of meters...)

shadows.csv
this file contains the shadows of all the buildings in buildings.csv, using the key height_mean as height data. The shadows represent the shadows at 2022-10-07T20:30:00 (GMT+1)
- geometry: same structure as in buildings.csv
- id: id of building the shadow belongs to.


nodes.csv
here you have all the nodes in the simplified street network (currently, this is only the drive network, the bike/foot network will look slightly different. But it will be a network nontheless.)
- osm_id: ID of the node in OSM (0 if node is helper (used to distinguish between self and multi-edges))
- lat: latitude of the node
- lon: longitude of the node
- vertex_id: id of the node in the graph (needed for edges)
- end: if the node can only be accessed from one direction (remnant of debugging. Fairly easy to infer from the network.)
- geopoint: Point in WKT Format. Duplicates lat and lon keywords (used in my code to transform the graph between different coordinate systems...)
- helper: whether the node is a helper node. (missing if not helper, else true...(equivalent to osm_id == 0))

edges.csv
these are the edges of the simplified graph. Each edge has a lot of data attached to it...
- osm_id: id of OSM way associated with this edge (not unique)
- geomlength: dummy data for the length of this edge.
- src_id: id of source vertex (references vertex_id)
- dst_id: id of destination vertex (references vertex_id) (be carefull about the order. The graph is directed.)
- geolinestring: linestring of the actual streets connecting the source and destination in WKT.
- shadow_frac: dummy data stating which fraction of the edge is in shadow.
- helper: whether the edge is a helper edge (missing if not, else true.)
here again, mainly depending on whether the edge is a helper or real, some of the other entries will be missing. Parse with care.

