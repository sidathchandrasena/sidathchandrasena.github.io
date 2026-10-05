---
preview_image:
excerpt:
---

## Entry 12: Contrails on the Busy SYD to MEL Route?

Today we're going to look at another region of interest I found earlier that also happens to be the busiest air route in the English speaking world: Sydney to Melbourne.

#### Joining the Dataset
I repeated the same steps as Entry 11 here, except that the contrails data was filtered for the SYD-MEL flight corridor instead of Western Australia.

Next, I filtered the dataset using the standingdata table to check whether each flight had origin/destination as SYD-MEL or MEL-SYD, or was a multi-stop route that included either of the two. In some cases, the route equivalent could not be found in the merge with standingdata and was labeled as unknown, resulting in the following:

```
route_group
other      20857
unknown     1913
MEL-SYD     1452
Name: count, dtype: int64
```

Next, I created a table that identified a "hit" as when contrail probability was greater than zero. This was separated into MEL-SYD and other flights and unknown, and what percentage of hits each category made.

```
	points	hits	share_of_hits	hit_rate	ci
route_group					
MEL-SYD	1452	34	23.129252	2.341598	0.777828
other	20857	107	72.789116	0.513017	0.096957
unknown	1913	6	4.081633	0.313643	0.250573
```



<br>
#### Visualising the Results
I first wanted to visualise how the MEL-SYD route compared to other flights in contrail at-risk regions.

```
	share_of_points	share_of_hits	hit_rate	over_under
route_group				
MEL-SYD	5.99	23.13	2.34	3.86
other	86.11	72.79	0.51	0.85
unknown	7.90	4.08	0.31	0.52
```

It is strange that MEL-SYD accounts for 6% of the total points, given how busy of a route it is.

On the left, we have the distribution of the "hits" (places where the point was in at-risk zone) amongst the MEL-SYD, other flights and unknown routes. On the right, we have how much of each group's sightings were "hits".



![Alt text](/assets/images/entry_12_mel_syd_hits.png)

So we can see that on the left plot, "other" flights account for a large portion of the hits. But that's also expected given that MEL-SYD is one out of many routes in that corridor. On the right however, we can see that the MEL-SYD route has a much higher amount of "hits". More analysis will need to be done to discover when this occurs. Is it at cruising altitude?

<br>

I thought it would be interesting to visually see what all of this looked like on a map. 

![Alt text](/assets/images/entry_12_mel_syd_map.png)

We can see two main trajectories between Melbourne and Sydney with points in contrail-forming air. Some questions to be asked here include: are these the most popular trajectories for this route and if so, why? What are some of the implications of changing this route, if it reduces the likelihood of entering contrail-laden areas?
