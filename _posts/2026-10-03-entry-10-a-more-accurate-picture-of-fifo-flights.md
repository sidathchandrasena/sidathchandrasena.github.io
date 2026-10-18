---
preview_image: entry_10_flights_by_carrier.png
description: Improving the filtered ADS-B dataset to classify Australian flights over Western Australia as FIFO vs non-FIFO using origin/destination and flight numbers.
---

## Entry 10: A More Accurate Picture of FIFO Flights

Last entry provided a glimpse into how Australia's mining industry plays a big role in our aviation industry. I also found this graphic<sup><a href="#ref1">1</a></sup> which shows a pretty cool visualisation of how FIFO flights look in the grand scale of things.

In my last analysis, I used some big assumptions to label flights as FIFO vs non-FIFO based on carrier. However, since then, I have come to see that this is not a great way to estimate if a flight is a FIFO service or not. While I don't plan to do anything particularly as complex as creating a model, I want to use this entry to get a more accurate depiction of FIFO scheduling and then hopefully use this dataset in my next entry with my Contrails data, which has continued to grow with my cron job.

<br>
### Filtering Data

This time, I used my ADSB data and filtered it using state boundaries to filter the dataset to Western Australia instead of the whole Australian continent.

<br>
```python
wa = states.loc[
    (states["admin"] == "Australia") & (states["name"] == "Western Australia"),
    "geometry",
].iloc[0]

gdf = gpd.GeoDataFrame(df, geometry=gpd.points_from_xy(df.longitude, df.latitude), crs="EPSG:4326")
df_wa = gdf[gdf.within(wa)]
```

<br>

I initially thought of using the registration number of the plane to filter data, but noticed that this can be inconsistent with the operating airline (e.g. if an airline sells the plane to another company). I also realised that callsign is not a great way to distinguish a carrier, as this seems to conflate airlines with its subsidiaries (e.g. Qantas + Network Aviation + QantasLink).

I decided to filter using icao24 which are 24-bit addresses using Australia's hex range, which is 7C0000–7FFFFF<sup><a href="#ref2">2</a></sup>. I also made sure to keep the earliest entry for that aircraft to remove duplicate entries.

I then merged this with OpenSky data on the icao24 column which gave me information such as: operator, owner, and aircraft. 


![Frequency of flights by operator in WA](/assets/images/entry_10_flights_by_carrier.png)

Interestingly, QLK callsign for Qantaslink is completely absent from WA due to the QantasLink brand being completely operated by Network Aviation there. Virgin Australia Regional Airlines and Virgin Australia are also indistinguishable by callsign.

<br>
### How to best distinguish FIFO vs non-FIFO?
After some confusion with operator codes, I decided that for the scope of this task, I would use flight number and origin/destination to determine if a flight was FIFO or non-FIFO.



<br>
#### Flight Number Rules
On major airlines:

**Virgin Australia:** Flights VA9000 to VA9999 are operated by Virgin Australia Regional Airlines and are listed as charter services<sup><a href="#ref3">3</a></sup> which I will take to be FIFO services for this exercise.

**Qantas:** QF1600–QF1999 are FIFO services<sup><a href="#ref4">4</a></sup> and QF2700–2735, QF2770–2775 and QF6590–6599 are all Network Aviation flight numbers<sup><a href="#ref5">5</a></sup>, which can be considered as FIFO here.

**Jetstar:** All flights can be labeled non-FIFO as it is a LCC carrier

**Alliance Airlines:** Could not find a clear designation of flight number being FIFO or not. Considering it's described as Australia's major FIFO operator, I am going to label all as FIFO despite it being a simplification.

**Rex (Regional Express):** all FIFO flights are operated via National Jet Express, so I labelled all ZL/RXA flights as non-FIFO<sup><a href="#ref6">6</a></sup>.

<br>
#### Origin/Destination Filtering
I used [standingdata](https://github.com/vradarserver/standing-data#CC0-1.0-1-ov-file) on Github's repo containing callsigns and their corresponding route (origin-destination) and then joined that with my dataset.

I located the airports of several confirmed mine sites using the ICAO codes: "YCHK", "YFDF", "YANG", "YBGD", "YWGA", "YIBO", "YGIA", "YSOL", "YEWA"

<br>
<br>
After doing both of these filtering actions, I got the following using pandas crosstab. 

Note that the "unknown" label was applied when the standingdata merge did not result in a match for the callsign so the origin or destination of a flight could not be determined, nor could the FIFO status be later determined from the flight number. "Unlabelled" is used when there was a flight number with a matching origin/destination but none of the flight number airline-specific rules were applied to them. I couldn't determine a flight number rule for Network Aviation to classify as FIFO/non-FIFO, which is a weakness as most flights are likely to be FIFO but outside of my mine-site airport list. "none" represents cases where the flight callsign failed to match with an airline in my list from Wikipedia of Australian airlines, which are mostly small Australian-registered aircraft which are private, business and small charter flights.

There are definitely a lot of limitations in the dataset, but let's see if there's an improvement to the plots:


```text
fifo_or_non_fifo      FIFO  non-FIFO  unknown  unlabelled
airline
Airnorth                 0         0       55         214
Alliance Airlines     1852         0        0           0
FlyPelican               0         0       21           0
Jetstar                  0      1979        0           0
National Jet Express  2157         0        0           0
Network Aviation      1747         0      667        4900
Qantas                  88      3572        0           0
Rex Airlines             0        31        0           0
Skytraders               0         0        2           0
Virgin Australia      2824      4789        0           0
none                     0         0     4522           0
```


Next, I plotted my plots from Entry 9 with the newly filtered and more accurate Entry 10 data to compare FIFO vs non-FIFO flights in day of week and time of day.

![FIFO vs non-FIFO in WA by hour of day compared with Entry 9](/assets/images/entry_10_comparison_time_of_day.png)

The effects are much more pronounced for FIFO flights, with a large majority of flights occurring early in the day or around 5pm. This is explained as the "morning wave", where workers arrive early to get to the site and work their 12-hour days by mid-morning<sup><a href="#ref7">7</a></sup>. The rise in the afternoon is explained by workers completing their shifts and returning to Perth where they commute home<sup><a href="#ref8">8</a></sup>. 


![FIFO vs non-FIFO in WA by day of week compared with Entry 9](/assets/images/entry_10_comparison_day_of_week.png)

Compared to Entry 9, there is a more visible split in proportion of weekend flights for FIFO compared to non-FIFO Saturday through Monday and then more FIFO flights occurring on weekdays. Weekend flights are uncommon for FIFO perhaps because weekends result in penalty rates for crew (higher expenses) and there are limited flights in regional areas on weekends.

<br>

It is exciting to work with the data directly on Jupyter Notebooks using Pandas but I don't want to go down a rabbithole in refining FIFO vs non-FIFO classification! I look forward to using this dataset next with the contrails dataset.

<br>
### References
<div class="references">
  <p><a name="ref1">[1]</a> r/perth (Reddit). <a href="https://www.reddit.com/r/perth/comments/14vnz06/air_traffic_around_australia_eight_hours/">Air traffic around Australia, eight hours.</a></p>
  <p><a name="ref2">[2]</a> AirLabs. <a href="https://airlabs.co/icao-24-bit-address">ICAO 24-bit address.</a></p>
  <p><a name="ref3">[3]</a> Hong Kong Airlines. <a href="https://www.hongkongairlines.com/en_CN/FFP/earn/airlines_partner-VA">Airline partner: Virgin Australia.</a></p>
  <p><a name="ref4">[4]</a> Australian Frequent Flyer. <a href="https://www.australianfrequentflyer.com.au/qantas-flight-numbers/">Qantas flight numbers.</a></p>
  <p><a name="ref5">[5]</a> Qantas Group. <a href="https://www.qantas.com/en-au/where-we-fly/network-aviation">Network Aviation.</a></p>
  <p><a name="ref6">[6]</a> Regional Express (Rex) (2022, September 30). <a href="https://www.rex.com.au/BlobViewer/BlobViewer.aspx?attachtype=MR&filename=6F6254563278466A6A474853692B6D756A31643639342F66716361714562457647676D6C7467465836774C6178415073444F78563470434E4B4B656F7533394B4E503158526358357A4F5A7769676F3646484E6F43673D3D">Rex finalises purchase of National Jet Express</a> [Media release].</p>
  <p><a name="ref7">[7]</a> Maisey, G., Cattani, M., Devine, A., Lo, J., Fu, S. C., & Dunican, I. C. (2022). <a href="https://doi.org/10.1016/j.apergo.2021.103617">Digging for data: How sleep is losing out to roster design, sleep disorders, and lifestyle factors.</a> <em>Applied Ergonomics</em>, 99, 103617.</p>
  <p><a name="ref8">[8]</a> Smithies, T. D., Vincent, G. E., Ferguson, S. A., Maisey, G., Fraser, H., & Dunican, I. C. (2026). <a href="https://doi.org/10.1093/annweh/wxag007">Drilling down on roster design: comparing fly-in fly-out roster patterns in the West Australian mining industry.</a> <em>Annals of Work Exposures and Health, 70</em>(2), wxag007.</p>
</div>
