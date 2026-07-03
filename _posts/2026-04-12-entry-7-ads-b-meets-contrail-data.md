---
preview_image: entry_7_geographic_risk.png
excerpt: Comparing the contrail risk data to the ADS-B data for autumn 2026.
---

## Entry 7: When Real Flights Meet Contrail Skies

Entries 5 and 6 had limited ADS-B (air traffic) data, resulting in the flight traffic data and contrail data being from different time periods: we had to overlay March 2026 OpenSky flight traffic data on winter 2025 contrail grids. 

In this post, both datasets cover the same eight-day window of **3-11 April 2026**, which will give us a more accurate picture of the relationship between contrail formation likelihood and air traffic in Australian skies.


### Datasets
#### Dataset A: `contrail_grids`

This is the Google Contrails API data. It splits Australia's airspace into squares about 28 km wide. Every 6 hours, a weather model assesses whether a contrail would form if a plane flew through there with a probability value between 0 and 1. My dataset only retains squares with a value greater than 0.1. 

The dataset is in autumn 2026, and a lot smaller in size compared to previous datasets during the winter 2025 timeframe. Ice supersaturated regions are a primary catalyst for contrails and winter in Australia experiences a greater frequency of ice supersaturation than autumn.

To summarise Dataset A:

| Attribute | Value |
|---|---|
| **Source** | Google Contrails API (ERA5-based forecast) |
| **Period** | 3–11 April 2026 |
| **Temporal resolution** | 6-hour intervals (00:00, 06:00, 12:00, 18:00 UTC) |
| **Spatial resolution** | 0.25° × 0.25° grid (≈28 km at Australian latitudes) |
| **Altitude range** | FL270–FL440 (18 levels) |
| **Total rows stored** | 234,817 cells (probability > 0.1) across 36 snapshots |


The figure below shows how each bar represents one 6-hour ERA5 snapshot.  

<figure>
  <img src="/assets/images/entry_7_dataset_a_overview.png" alt="Dataset A overview">
  <figcaption>Bar Plot of Contrail Grid Dataset</figcaption>
</figure> 


#### Dataset B - `flight_snapshots`

Dataset B uses ADSB.fi, a free alternative to OpenSky that provides community-driven and crowdsourced air traffic data.

The data was collected using a Github workflow titled `collect-flights.yml`, which runs a script called `collect_flights_adsb.py` every 30 minutes. ADSB.fi then returns a big JSON blob with one aircraft per entry. The script filters to only accept aircraft above FL200 (20,000 ft), making sure they are airborne.


| Attribute | Value |
|---|---|
| **Source** | ADS-B.fi open API (12 tiling circles) |
| **Period** | 3–11 April 2026 |
| **Poll frequency** | Every ~30 minutes (472 polls over the window) |
| **Total rows** | 30,150 |
| **Unique aircraft** | 1,560 distinct ICAO hex addresses |
| **Altitude filter** | FL200 and above only |

The plot below illustrates Dataset B, air traffic over time in Australian skies. The troughs represent AEST night (~ UTC 12:00-22:00) when fewer aircraft are at cruise altitude. Daily peaks coincide with the busy late-morning domestic period.

![Dataset B overview](/assets/images/entry_7_dataset_b_overview.png)


----


### Let's combine both plots together.
This way, we can investigate: how much of all the aircraft in Australian skies is exposed to contrail-risk regions at one time? Each snapshot captures how many aircraft were inside an active contrail risk cell at a point in time. The blue dashed-line shows the total aircraft in the sky at each poll and the red line shows how many of those aircraft were at risk. 

For most of the 8-day window, the red and blue lines are far apart. The only exception is on 5-6th of April, where the blue line stays on its steady daily rhythm while the red line is noticeably higher, indicating that something occurred here that was atmosphere-driven. To better understand what caused this, we would need to look at weather data.

![Fleet exposure over time](/assets/images/entry_7_exposure_timeseries.png)

---

### Where does this risk occur (on an Australian map)?

The heatmap placed over the map of Australia below shows the **Compound Risk Score (CRS) = total aircraft observations in that cell x mean contrail probability**, meaning cells where aircraft flew through active contrail risk.

There are some notable clusters of purple - the Western Australia interior and Bass Strait. The Tasman Sea to the right also showed some intersection of contrail risk and flight traffic to a lesser extent than seen earlier in Entry 5.

![Compound risk geography](/assets/images/entry_7_geographic_risk.png)


----

### What time of day is contrail risk the highest?
The below plot combines 8 days worth of data into one single 24-hour UTC clock that identifies at what time contrail exposure peaks.

![Diurnal pattern](/assets/images/entry_7_diurnal.png)

The blue line (left axis) shows the average number of aircraft in the sky at each UTC hour. This shows a low at AEST night and a high across the daytime domestic period (peaking around UTC 0:00, which is ~10:00 AEST).

The red dashed line (right axis) shows the average percentage of active fleet inside a contrail risk zone at each UTC hour. This peaks at UTC 15:00 (01:00 AEST), so the middle of Australian overnight and has a low at UTC 01:00-02:00 (10:30 AEST).

The key finding here is that the hours with the most air traffic (UTC 22:00-02:00, morning to midday AEST) are not the most dangerous for contrail exposure, but rather the quieter overnight window.

----

## Questions + Takeaways
**Takeaways:**
* In addition to the Bass Strait and Tasman Sea, another area of interest identified for contrail formation risk is the interior of WA.

* The overnight window is when the highest risk of contrail formation occurs rather than the hours when there is the most air traffic.
  
**Food for thought for the next entry:** 

* What caused that increase in flights at-risk of contrail formation in Plot C? 
  
* How does this dataset from April compare to other months? 

* What weather data can we use to understand the changes in at-risk aircraft? 
  
* What causes the WA interior, Bass Strait and Tasman Sea to have a higher risk of contrail formation?
