---
preview_image:
excerpt:
---

## Entry 9: Exploring FIFO Charter Flights

Last time, my discoveries into Interior WA, an area I knew little about, revealed a large number of flights for FIFO work. In this entry, I plan to take a glance at what FIFO work is, and how it's represented in my growing dataset.

<br>
### What is FIFO work?

Growing up in South Australia, I was familiar with the term FIFO meaning "fly-in, fly-out" and its association with the mining industry with places such as Olympic Dam listed as destinations on small regional flights. In terms of those who work in the FIFO industry, what comes to mind is people who wear hardhats and hi-vis vests, but I imagined all sorts of people connected to the industry including site engineers, tradesmen and executives.

<figure>
  <img src="/assets/images/entry_9_fifo_plane.jpg" alt="A description of the image">
  <figcaption>"Alliance Airlines | Fokker 100 VH-FKK" by Luke McConville is licensed under CC BY-NC-ND 2.0. To view a copy of this licence, visit https://creativecommons.org/licenses/by-nc-nd/2.0/?ref=openverse.</figcaption>
</figure>

Some preliminary research reveals that FIFO work takes place primarily in QLD and WA as these states have a lot of mining sites and involves specialists coming into the site to do work over a rostered period. Some of the roles include engineers, electricians, security guards, nurses, chefs, etc<sup><a href="#ref3">3</a></sup>. 

I was curious why people would choose to repeatedly commute instead of relocating, which could provide a lot of opportunities for the area near the mining site. An article by Windle and Rolfe<sup><a href="#ref5">5</a></sup> revealed that salaries would need to be increased significantly for someone to relocate instead of FIFO work. It seems that the Australian mining industry is very reliant on this kind of work.

From one article<sup><a href="#ref4">4</a></sup>, I found some examples of the typical rosters FIFO workers take on:
*  8/6 (work eight days, have six days off)
*  2/1 (works two weeks, one week off)
*  4/2 (work four weeks, then two weeks off)
*  Even time: equal time on-site and off-site

To gain a better idea of what FIFO scheduling looks like, I decided to look at my own dataset. In Entry 8, we can see that there was a carrier called "Network Aviation", which I found is a Perth-based carrier that provides scheduled and charter services for FIFO workers.<sup><a href="#ref6">6</a></sup>

<br>
### Exploring FIFO Scheduling with Pandas

My goal is to see how scheduling or flight frequency differs between regional FIFO carriers and the major carriers flying between cities. 

In my Jupyter notebook, I imported my ADSB.fi traffic data into a Pandas dataframe. I then filtered the data to Australian airspace using a NaturalEarth polygon, and filtered to only Australian carriers using the list of Australian airlines and their ICAO codes on Wikipedia.

I realised that several factors would need to be considered to determine whether a flight was a FIFO flight: terminal, flight number and some airlines may do both FIFO and intercity routes (e.g. Virgin Australia).

For now, what I chose to do is to pick three airlines that do largely intercity routes (Qantas, Virgin Australia, Jetstar) and three carriers that largely operate FIFO services (Network Aviation, National Jet Express and Alliance Airlines).

Below is a snippet of the resulting Pandas dataframe:

```python
df_au.head(2)
```

```
   id  snapshot_id                       collected_at  icao24 callsign   latitude  longitude  baro_altitude_m  geo_altitude_m  flight_level  ...                    geometry callsign_prefix   airline iata icao callsign_wiki   category      hub_airports                          notes carrier_group
0   1    1775190223  2026-04-03 04:23:43.885157+00:00  7c045e   JST985 -34.890297  146.40404         11277.60        11704.32        370.00  ...  POINT (146.40404 -34.8903)              JST  Jetstar   JQ  JST       JETSTAR  scheduled  Melbourne Airport  Low-cost subsidiary of Qantas       non_fifo
1   2    1775190223  2026-04-03 04:23:43.885157+00:00  7c4a51   JST633 -35.001617  146.46642          9745.98        10142.22        319.75  ...  POINT (146.46642 -35.00162)             JST  Jetstar   JQ  JST       JETSTAR  scheduled  Melbourne Airport  Low-cost subsidiary of Qantas       non_fifo
```

<br>
#### Plot 1: FIFO vs non-FIFO - Hour of Day 
For the first plot, I wanted to view how FIFO vs non-FIFO flights frequency differs in time of day, keeping in mind that ADSB.fi data is real-time traffic rather than scheduled departure. So I grabbed the earliest seen point of each flight and got the following plot.

![Alt text](/assets/images/entry_9_fifo_hour_of_day.png)

FIFO flights compared to non-FIFO peak around 7am and 5pm. Before 7am, non-FIFO flights dominate, between 10am-2pm and 6-8pm. This seems to align well with many (paywalled) articles online depicting how extremely early FIFO flights have led to some health concerns over poor sleep quality and risks of car accidents to and from the airport. One example of an article that explores this is Maisey et al<sup><a href="#ref7">7</a></sup>.

<br>
#### Plot 2: FIFO vs non-FIFO - Day of the Week
I also wanted to see whether the above mentioned rosters may be reflected when plotted against the days of the week.

![Alt text](/assets/images/entry_9_fifo_day_of_week.png)

Very interesting. Here we can see that FIFO flights consistently make a higher proportion of air traffic in Australian airspace Mon-Thurs, and then at the end of the week (Fri-Sun), commercial flights make up a much higher proportion.

Looking at information online on FIFO flights and which days on the week they are: one website<sup><a href="#ref8">8</a></sup> mentions how you fly out at the beginning of the week, Sunday or Monday and return in two weeks or so repeatedly - on a 2:1 schedule. There may be other factors that contribute to the above plot's shape, such as whether workers fly in their own time or in company time, as this would factor into things such as penalty rates on weekends.

<br>

### Reflections
Learning about FIFO work from an aviation point of view was intriguing and I see it as something to further look into. Today's analysis also raises some questions and thoughts:

* Which FIFO routes are more suspectible to crossing regions likely to form contrails?
* How does aircraft type affect contrail formation, given that FIFO flights are considerably smaller than intercity flights?
* How does the frequency of FIFO flights vary seasonally?

<br>

### References

<div class="references">
  <p><a name="ref1">[1]</a> <a href="https://lens.monash.edu/covid-19-fifo-workers-and-the-risk-facing-remote-mining-communities/">COVID-19, FIFO workers, and the risk facing remote mining communities.</a></p>
  <p><a name="ref2">[2]</a> PLOS ONE, Edited by Edward Jay Trapido, vol. 17, issue 10, p. e0275008. <a href="https://ui.adsabs.harvard.edu/link_gateway/2022PLoSO..1775008A/doi:10.1371/journal.pone.0275008">https://ui.adsabs.harvard.edu/link_gateway/2022PLoSO..1775008A/doi:10.1371/journal.pone.0275008</a></p>
  <p><a name="ref3">[3]</a> <a href="https://www.bravusmining.com.au/what-is-fifo-work-in-australia/">FIFO work — Bravus Mining</a></p>
  <p><a name="ref4">[4]</a> Tim D Smithies, Grace E Vincent, Sally A Ferguson, Gemma Maisey, Holly Fraser, Ian C Dunican, <a href="https://doi.org/10.1093/annweh/wxag007">Drilling down on roster design: comparing fly-in fly-out roster patterns in the West Australian mining industry</a>, Annals of Work Exposures and Health, Volume 70, Issue 2, March 2026, wxag007</p>
  <p><a name="ref5">[5]</a> Windle, J., & Rolfe, J. (2013). Using discrete choice experiments to assess the preferences of new mining workforce to commute or relocate to the Surat Basin in Australia. Resources policy, 38(2), 169-180.</p>
  <p><a name="ref6">[6]</a> Qantas Group. https://www.qantas.com/en-au/where-we-fly/network-aviation </p>
  <p><a name="ref7">[7]</a> Maisey, G., Cattani, M., Devine, A., Lo, J., Fu, S. C., & Dunican, I. C. (2022). Digging for data: How sleep is losing out to roster design, sleep disorders, and lifestyle factors. Applied Ergonomics, 99, 103617. </p>
  <p><a name="ref8">[8]</a> Programmed by Persol. Working FIFO in Australia: Pay, Rosters, Lifestyle and What to Expect. https://skilled.programmed.com.au/working-fifo-australia-pay-rosters-lifestyle-what-to-expect/ </p>
</div>