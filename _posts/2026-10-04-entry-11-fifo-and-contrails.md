---
preview_image:
excerpt:
---

## Entry 11: Do FIFO flights enter contrail-laden regions?

Now that I was able to filter the ADS-B dataset to WA and label flights as FIFO and non-FIFO, it's time to join the dataset with the Google Contrails dataset. From this, I want to now see what percentage of flights that are in the "risk region" for contrail formation.

#### Joining the Dataset
The contrails dataset has coordinates that are rounded to 0.25, so the ADS-B data had to first be rounded so there could be a match. So this was done to the longitude and latitude columns. Flight level was also rounded to the nearest 10 to match the contrails dataset. This was also done for the timestamp column, which in the contrails table only records results for each multiple of 6 (UTC 00, 06, 12, 18), so each result is rounded to one of these four times to match.

Next, the ADS-B data was filtered between FL270 and FL440 as this was the range of the contrails dataset, which is the altitude at which contrails are most likely to form.

Next, the rounded timestamp column was filtered in the ADS-B dataset to make sure it matches the timestamps in the contrails dataset.

Then the rounded matching columns in the ADS-B dataset were renamed to match the contrails dataset and a left merge was performed.

The `fillna()` command was run on `contrails` for rows where there was no match, as this indicates a contrail forcing index of zero. Then, the combined dataset was filtered for cases where a CFI > 0.001 was found so we can look at "at-risk" observations.

Next, I created a new variable called `local_date` to convert the timestamp from UTC to Perth time.

Next, I imported the FIFO dataset labeled from last entry and then performed an inner merge the get this merged table to have labeling. This left the resulting table with only Australian airlines and one sighting per labelled flight.

<br>
#### Visualising the Results
I first wanted to see how many of the flights in the dataset that pass through contrail-forming air are FIFO or non-FIFO by percentage.

![Flights that passed through contrail-forming air FIFO vs non-FIFO](/assets/images/entry_11_contrail_distribution.png)

The plot accounts for the different frequencies between non-FIFO and FIFO flights. Considering non-FIFO flights are almost double in frequency, this is an intriguing result.

<br>

The next plot shows the altitude distribution of among FIFO and non-FIFO flights. The right-hand plot shows when you filter out observations that were rounded to beyond 1 hour of the recorded time (given contrails data was every 6 hours, this accounts for a big disparity between the observation and time. However, on the other hand, this means a significantly smaller number of observations).

![FIFO vs non-FIFO by Altitude](/assets/images/entry_11_altitude_distribution.png)

As seen above, non-FIFO flights dominate at altitudes beyond FL340 and contrail-prone altitudes as well. Maybe due to FIFO flights having small aircraft? Let's verify that.

<br>
![FIFO vs non-FIFO routes by frequency](/assets/images/entry_11_aircraft_distribution.png)

Let's compare the difference between the cruising altitude of a Fokker 100 with a Boeing 737-800. Fokker 100 has a max cruising altitude of 35,000 ft ([source](https://aeropedia.com.au/content/fokker-100/)) while Boeing 737-800 has a max cruising altitude of 41,000 ft ([source](https://www.rocketroute.com/aircraft/boeing-737-800)). This explains a lot.


<br>
Now let's look at the aircraft distribution amongst FIFO and non-FIFO flights in at-risk regions. 

![Comparing Aircraft Type FIFO vs non-FIFO](/assets/images/entry_11_fleet_distribution.png)

What I find to be interesting here is the big difference the number of observations between the top and bottom bar of FIFO and non-FIFO, despite being the same aircraft. Also the Boeing 737-MAX at second from top, and then Boeing 737-800 below, is that to do with the increased adoption of MAX aircraft in domestic Australia? 

Perhaps this should be taken with some skepticism, however, as the right-hand plot demonstrates that when the data is filtered to within one hour of the observation, it seems that the diversity in aircraft type is significantly reduced.

<br>
#### Concluding Remarks
These findings can take us in many directions. What I am most curious from today's results is identifying which FIFO aircraft are responsible for those results in the contrail risk zone. I am guessing that they are those flights in those planes that match the common planes for non-FIFO services, such as Airbus A320s and Boeing 737-800s. From this I wonder: what prompts a FIFO service or charter to be done in a large passenger jet as opposed to a small one? What are those routes? What are the implications of a shift in aircraft operation on the mining industry and the FIFO workforce?

I plan to take a small break from looking at FIFO work to compare this to another region of interest identified in a previous entry: the infamous Sydney-Melbourne route.
