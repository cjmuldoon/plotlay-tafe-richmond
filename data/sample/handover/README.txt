Building handover: Sample campus, drawn out (sample data)
Made with Plotlay, 2026-11-08 00:00 UTC

39 buildings, 1 level, 0 rooms, 0 zones, 372 register assets of 23 types in 6 systems, 0 doors and 0 windows.

Files
COBie.xlsx       COBie 2.4 (BS 1192-4) workbook: Facility, Floor, Space, Zone, Type, Component, System, Job, Attribute, Coordinate.
model.ifc        IFC4: a storey per level, rooms as spaces, assets as IFC elements with types, zones, systems, Plotlay ids.
haystack.json    Project Haystack 4 records as a Hayson grid: site, floor, space, equip with siteRef, spaceRef, floorRef.
haystack.zinc    The same records in Zinc.
brick.ttl        Brick Schema (Turtle): building, floors, rooms, zones and equipment with hasPart, isPartOf, hasLocation, feeds.

Ids
Every building, level, room, zone and asset keeps the same id from one export to the next. The ids are UUIDs;
IFC and COBie (ExtIdentifier) use the same 128 bits as a 22-character IFC GlobalId, Haystack ids are the UUIDs,
Brick names are urn:plotlay:<project>/<uuid>, IMDF feature ids are the UUIDs. Import the new pack over the old one
and your system updates what it has instead of adding it again.

Accuracy depends on the drawings, imagery and measurements the project was made from. Register standards and
inspection frequencies are typical: verify against the current standard and the building’s maintenance schedule.

Left out
dxf: CAFM DXF needs rooms.
svg: SVG floor plans needs rooms.
imdf: IMDF indoor map needs rooms.
gbxml: gbXML needs rooms.

Notes
Buildings from the Sample data: buildings data layer: each key a building, the register assets in the building whose outline holds them.
