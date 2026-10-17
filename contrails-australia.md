---
layout: home
title: Contrails Australia
permalink: /contrails-australia/
noindex: true
---

<div class="hero-banner" style="background-image: url('/assets/images/gabriela-natiello-_ciDodax1Ks-unsplash.jpg');"></div>
<p class="photo-credit">Photo by <a href="https://unsplash.com/@gabrielanatielloartgallery?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Gabriela Natiello</a> on <a href="https://unsplash.com/photos/an-airplane-is-flying-in-the-blue-sky-_ciDodax1Ks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a></p>

## Welcome!
What are those white lines in the air next to planes? This website will document my findings as I look at where, when and why contrails form in Australian skies.

<div class="summary-card" markdown="1">
<p class="summary-card-title">A summary of my findings so far</p>

**Research question:** Where and when do flights in Australian skies pass through contrail-forming air, and what would avoiding it involve for regional and FIFO charter flights?

**Data:** Contrail forecasts from the Google Contrails API (a 0–4 severity index from a weather-forecast model) for winter 2025 and April 2026, matched with ADS-B flight tracks from ADSB.fi and BITRE domestic route statistics.

**Key findings:**
* The contrail forcing index over Australian skies in winter 2025 was highest overnight (14 UTC, midnight AEST) and lowest around midday (02 UTC): [Entry 3]({% post_url 2026-03-09-entry-3-contrail-probabilities-with-respect-to-latitude-altitude-time %}). Matched flight data from April 2026 showed the same overnight pattern, with the share of aircraft in contrail-forming air peaking at 15 UTC: [Entry 7]({% post_url 2026-04-12-entry-7-ads-b-meets-contrail-data %})
* Some regions of interest were identified when pairing contrail forcing index with air traffic data: Hobart to Melbourne over the Bass Strait (the most contrail-prone route per flight, 2.5x Melbourne to Sydney), the Melbourne to Sydney corridor and interior WA: [Entry 5]({% post_url 2026-03-15-entry-5-adding-in-flight-data %}), [Entry 6]({% post_url 2026-04-03-entry-6-domestic-flight-routes %}), [Entry 7]({% post_url 2026-04-12-entry-7-ads-b-meets-contrail-data %}), [Entry 8]({% post_url 2026-07-11-entry-8-examining-interior-wa %})
* In WA, 2.1% of fly-in, fly-out (FIFO) flights passed through contrail-forming air compared with 2.9% of non-FIFO flights, which may be due to the smaller aircraft used for many FIFO operations: [Entry 10]({% post_url 2026-10-03-entry-10-a-more-accurate-picture-of-fifo-flights %}), [Entry 11]({% post_url 2026-10-04-entry-11-fifo-and-contrails %})
* On the Melbourne to Sydney corridor, Melbourne–Sydney flights were in contrail-forming air at about 3.9x the rate their share of traffic would predict: [Entry 12]({% post_url 2026-10-05-entry-12-mel-to-syd %})

**Limitations:** Short data windows, rough FIFO labelling, the Google Contrails data is the output of a model rather than data containing actual contrails.

**Next steps:** looking at small altitude or departure-time changes for at-risk FIFO flights, improved labeling of FIFO vs non-FIFO flights, looking at altitude changes in main routes for Melbourne-Sydney route, matching traffic data and contrails data for the Hobart to Melbourne route.

</div>

The entries below show the findings in more detail, from first look at the data to the latest analysis.
