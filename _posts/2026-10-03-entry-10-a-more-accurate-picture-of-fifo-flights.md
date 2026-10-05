---
preview_image:
excerpt:
---

## Entry 10: A More Accurate Picture of FIFO Flights

Last entry provided a glimpse into how Australia's mining industry plays a big role in our aviation industry. I also found [this graphic](https://www.reddit.com/r/perth/comments/14vnz06/air_traffic_around_australia_eight_hours/) which shows a pretty cool visualisation of how FIFO flights look in the grand scale of things.

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

I decided to filter using icao24 which are 24-bit addresses using Australia's hex range, which is 7C0000–7FFFFF<sup><a href="#ref1">1</a></sup>. I also made sure to keep the earliest entry for that aircraft to remove duplicate entries.

I then merged this with OpenSky data on the icao24 column which gave me information such as: operator, owner, and aircraft. 


![Frequency of flights by operator in WA](/assets/images/entry_10_flights_by_carrier.png)

Interestingly, QLK callsign for Qantaslink is completely absent from WA due to the QantasLink brand being completely operated by Network Aviation there. Virgin Australian Regional Airlines and Virgin Australia are also indistinguishable by callsign.

<br>
### How to best distinguish FIFO vs non-FIFO?
After some confusion with operator codes, I decided that for the scope of this task, I would use flight number and origin/destination to determine if a flight was FIFO or non-FIFO.



<br>
#### Flight Number Rules
On major airlines:

**Virgin Australia:** Flights VA9000 to VA9999 are operated by Virgin Australia Regional Airlines and are FIFO services<sup><a href="#ref2">2</a></sup>.

**Qantas:** QF1600–QF1999 are FIFO services<sup><a href="#ref3">3</a></sup>.

**Jetstar:** All flights can be labeled non-FIFO as it is a LCC carrier

**Alliance Airlines:** Could not find a clear designation of flight number being FIFO or not. Considering it's described as Australia's major FIFO operator, I am going to label all as FIFO despite it being a simplification.

**Rex (Regional Express):** all FIFO flights are operated via National Jet Express, so all ZL/RXA flights can be labeled as non-FIFO<sup><a href="#ref4">4</a></sup>.

<br>
#### Origin/Destination Filtering
I used [standingdata](https://github.com/vradarserver/standing-data#CC0-1.0-1-ov-file) on Github's repo containing callsigns and their corresponding route (origin-destination) and then joined that with my dataset.

I located the airports of several confirmed mine sites using the ICAO codes: "YCHK", "YFDF", "YANG", "YBGD", "YWGA", "YIBO", "YGIA", "YSOL", "YEWA"

<br>
<br>
After doing both of these filtering actions, I got the following using pandas crosstab. There are definitely a lot of limitations in the dataset, but let's see if there's an improvement to the plots:


```text
fifo_or_non_fifo        FIFO  non-FIFO  unknown  unlabelled
airline
Airnorth                  0        0       55        213
Alliance Airlines       351     1475        0          0
FlyPelican                0        0       20          0
Jetstar                   0     1960        0          0
National Jet Express   2138        0        0          0
Network Aviation       1728        0      660       4842
Qantas                   84     3544        0          0
Rex Airlines              0        0        3         28
Skytraders                0        0        2          0
Virgin Australia       2786     4745        0          0
none                      0        0     4474          0
```


Next, I plotted my plots from Entry 9 with the newly filtered and more accurate Entry 10 data to compare FIFO vs non-FIFO flights in day of week and time of day.

![FIFO vs non-FIFO in WA by hour of day compared with Entry 9](/assets/images/entry_10_comparison_time_of_day.png)

The effects are much more pronounced for FIFO flights, with a large majority of flights occurring early in the day or around 5pm. This is explained as the "morning wave", where workers arrive early to get to the site and work their 12-hour days by mid-morning<sup><a href="#ref5">5</a></sup>. The rise in the afternoon is explained by workers completing their shifts and returning to Perth where they commute home<sup><a href="#ref6">6</a></sup>. 


![FIFO vs non-FIFO in WA by day of week compared with Entry 9](/assets/images/entry_10_comparison_day_of_week.png)

Compared to Entry 9, we see a more visible split in proportion of weekend flights for FIFO compared to non-FIFO Saturday through Monday and then more FIFO flights occurring on weekdays. Weekend flights are uncommon for FIFO perhaps because weekends result in penalty rates for crew (higher expenses) and there are limited flights in regional areas on weekends.

<br>

It is exciting to work with the data directly on Jupyter Notebooks using Pandas but I don't want to go down a rabbithole in refining FIFO vs non-FIFO classification! I look forward to using this dataset next with the contrails dataset.

<br>
### References
<div class="references">
  <p><a name="ref1">[1]</a> AirLabs. <a href="https://airlabs.co/icao-24-bit-address">ICAO 24-bit address.</a></p>
  <p><a name="ref2">[2]</a> Hong Kong Airlines. <a href="https://www.hongkongairlines.com/en_CN/FFP/earn/airlines_partner-VA">Airline partner: Virgin Australia.</a></p>
  <p><a name="ref3">[3]</a> Australian Frequent Flyer. <a href="https://www.australianfrequentflyer.com.au/qantas-flight-numbers/">Qantas flight numbers.</a></p>
  <p><a name="ref4">[4]</a> Wikipedia. <a href="https://en.wikipedia.org/wiki/National_Jet_Express">National Jet Express.</a></p>
  <p><a name="ref5">[5]</a> Medical Xpress (2021). <a href="https://medicalxpress.com/news/2021-10-fly-in-fly-out-workers-significant-loss.html">Fly-in fly-out workers suffer significant loss of sleep.</a></p>
  <p><a name="ref6">[6]</a> PubMed Central. <a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC13016775/">https://pmc.ncbi.nlm.nih.gov/articles/PMC13016775/</a></p>
</div>
