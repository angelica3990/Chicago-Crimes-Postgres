# Chicago-Crimes-Postgres
---

## Overview

This assignment uses Python, SQL, PostGIS, and Folium to analyze Chicago crime data from 2016 to 2018. The dataset comes from the City of Chicago Data Portal and contains roughly 6.5 million records. The analysis focuses on mapping crime patterns by district, identifying gun crime hotspots, and using geospatial queries to find crimes farthest from police stations. All maps are interactive Choropleth maps built with Folium.

The data ran through a PostgreSQL + PostGIS database served via Docker. The Boundaries.geojson file provided district boundary shapes for the Choropleth layers.

---

## Key Takeaways

**Overall Crime by District**
- Crime counts varied significantly across Chicago's 22 police districts.
- The Choropleth map made it easy to see which districts carry the highest total crime burden.

**Violent Crime Distribution**
- Violent crimes (Theft, Assault, Battery, Robbery, Kidnapping, Sexual Assault, Murder) were mapped by district with popup tables showing the breakdown per type.
- Battery and Theft consistently appeared as the most common violent crime categories across districts.

**Gun Crime Hotspots**
- Gun-related violent crimes were isolated and mapped separately. Districts in the south and west sides of Chicago showed darker shading, indicating higher gun crime concentrations.
- The block with the highest gun crime count in each district was identified. Marker size on the map was scaled to the crime count, making the worst blocks immediately visible.

**Crime Density**
- Crime density was calculated as crimes per 100 hectares using district area derived from the GeoJSON boundaries. This normalized the raw count data so smaller, denser districts could be compared fairly against larger ones.

**Gun Crime Arrests vs No Arrests**
- Green markers indicate gun crimes that resulted in an arrest. Red markers indicate those that did not.
- The clustering showed that the majority of gun crime incidents did not result in an arrest, and arrests were not evenly distributed across districts.

**Location Context: Street vs Residence**
- Gun crimes were also broken down by location description. Street-level gun crimes and residence-level gun crimes were plotted in separate marker clusters.
- Street crimes were more common overall, but residence-based gun crimes were notable in certain districts.

**Farthest Gun Crimes from Police Stations**
- Using ST_Distance in PostGIS, the farthest gun crime from the police station was identified in each district. This is useful for thinking about response time gaps and resource placement.
- Some districts had gun crimes occurring very far from the nearest station, which could inform future station placement decisions.

**Violent Crime Trends (365-Day Rolling Average)**
- A rolling average trend chart showed how each violent crime type changed over the 2016 to 2018 period.
- Battery was the highest-volume violent crime by a wide margin. Theft followed as second. Kidnapping and murder were comparatively low but still measurable.

**Peak and Low Crime Days**
- Tuesday had the most crime reports across the full dataset.
- Saturday had the fewest.
- This has direct staffing implications. Scheduling more resources mid-week rather than on weekends aligns with when incidents are most likely to occur.

---

## Notes

- Libraries used: psycopg2, pandas, folium, folium.plugins.MarkerCluster, matplotlib, json, area.
- PostGIS functions used: ST_Distance, ST_AsText, ST_X, ST_Y.
- District area was calculated using the area library applied to GeoJSON geometry objects.
- A WITH clause (CTE) and RANK() window function were used to find the top block per district for gun crimes.
- The unit testing database (chicago_crimes_ut) was used during development. Final submission runs on the full chicago_crimes database.
