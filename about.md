---
layout: page
title: About The Project
permalink: /about/
noindex: true
---
<script>document.body.classList.add('contrails-theme');</script>

<details class="accordion" open>
  <summary>What are contrails?</summary>
  <div class="accordion-body">
    <p>Contrails (or condensation trails) are those white lines you see in the sky when planes fly overhead. They form when hot, humid air from jet engine exhaust mixes with the extremely cold air at high altitudes, turning water vapour into ice crystals.</p>
    <p>How long a contrail lingers depends on surrounding atmospheric conditions. In dry air it evaporates quickly; in humid air the ice particles grow, and the contrail can persist and spread for hours.</p>
  </div>
</details>

<details class="accordion">
  <summary>Why contrails matter</summary>
  <div class="accordion-body">
    <p>Aircraft are responsible for around 2% of total global warming<sup><a href="#ref1">1</a></sup> and the aviation industry continues to grow post-pandemic. CO2 emissions make up most of the exhaust, but contrails from water vapour make up a majority of non-CO2 warming.</p>
    <p>Contrails do this by trapping heat in the atmosphere, with some estimates putting their warming effect on par with or exceeding that of aviation's direct carbon emissions<sup><a href="#ref2">2</a></sup>. As air traffic continues to grow, so does the scale of this effect. Recent research has complicated the picture further: warming estimates vary widely, and contrails forming inside existing cloud cover may behave very differently to those in clear skies. There is still a lot we don't fully understand.</p>
  </div>
</details>

<details class="accordion">
  <summary>Why focus on Australia?</summary>
  <div class="accordion-body">
    <p>Most contrail research has focused on Europe and North America, which are regions with dense air traffic and well-studied atmospheres. Australia is comparatively underexplored, despite having a unique atmospheric environment and a heavy reliance on long-haul air travel due to its geographic isolation. Google's Project Contrails, for example, hopes to extend its models to places in Oceania in the future.<sup><a href="#ref3">3</a></sup></p>
  </div>
</details>

<details class="accordion">
  <summary>Fun facts about contrails</summary>
  <div class="accordion-body">
    <p>Did you know that:</p>
    <ul>
      <li>Non-CO<sub>2</sub> effects, which include contrails, may account for as much as <strong>two-thirds of aviation's climate impact</strong>, as found in a 2018 study.<sup><a href="#ref2">2</a></sup></li>
      <li>Contrails are estimated to be responsible for up to around <strong>57% of global aviation's net effective radiative forcing</strong>.<sup><a href="#ref4">4</a></sup></li>
      <li>In 2019, around <strong>14% of flights formed a contrail with a warming effect</strong>, and only around <strong>2% of all flights created ~80% of warming</strong>.<sup><a href="#ref5">5</a></sup></li>
      <li>The same study found that in 2019, <strong>more than 50% of global contrail effects were concentrated over Europe, North America and the North Atlantic</strong>.<sup><a href="#ref5">5</a></sup></li>
    </ul>
  </div>
</details>

<details class="accordion">
  <summary>What this project seeks to find</summary>
  <div class="accordion-body">
    <p>My research questions continue to evolve as I create each new post and gain more knowledge. Broadly, this project focuses on understanding which regions in Australian skies have the highest crossover in air traffic and where contrail formation is most likely, and if they coincide. I would also like to look at what routes and types of flights have the most exposure to contrails and what types of strategies can be used to avoid contrails such as small changes to altitude or departure time and the operational trade-offs that come with them. I use different parts of Australia's aviation network as case studies, with a mix of regional and intercity routes which are shaped by Australia's different geography compared to US/Europe.</p>
  </div>
</details>

<details class="accordion">
  <summary>About the author</summary>
  <div class="accordion-body">
    <img src="/assets/images/sidath.jpg" alt="Sidath Chandrasena" class="author-photo">
    <p>My name is <strong>Sidath Chandrasena</strong>. I am a recent graduate with a dual degree in Integrative Engineering and International Studies with a minor in Data Science from Lafayette College. I work as a software engineer and this project combines my background in data and my long-standing interest in aviation, the environment and interdisciplinary work. </p>
    
    <p>The analysis is built on Python, PostgreSQL, and GeoPandas, using data from the Google Contrails API<sup><a href="#ref6">6</a></sup>, ADSB.fi<sup><a href="#ref7">7</a></sup>, OpenSky Network<sup><a href="#ref8">8</a></sup> and the Bureau of Infrastructure and Transport Research Economics (BITRE)<sup><a href="#ref9">9</a></sup>.</p>

    <p>As someone who grew up in Australia, I take a lot of interest in how Australia's unique geography plays a major role in its reliance on long-haul and domestic air travel to stay connected to the rest of the world, and how it is relatively unexplored compared to places in the Northern Hemisphere.</p>

    <p><strong>Contrails Australia</strong> is my personal project to examine contrail formation in Australian skies using real atmospheric data by identifying hotspots, understanding the conditions that create them and exploring what avoidance might look like in practice.</p>
    <ul>
      <li><strong>GitHub:</strong> <a href="https://github.com/sidathchandrasena">sidathchandrasena</a></li>
      <li><strong>LinkedIn:</strong> <a href="https://www.linkedin.com/in/sidath-chandrasena">sidath-chandrasena</a></li>
      <li><strong>Email:</strong> sidath.chandrasena[at]gmail.com</li>
    </ul>
  </div>
</details>

<details class="accordion">
  <summary>References</summary>
  <div class="accordion-body references">
    <p><a name="ref1">[1]</a> RMI — <a href="https://rmi.org/aviation-contrails-what-we-know-and-what-we-dont-about-this-warming-phenomenon/">Aviation Contrails: What We Know and What We Don't About This Warming Phenomenon</a></p>
    <p><a name="ref2">[2]</a> Lee, D. S., Fahey, D. W., Skowron, A., et al. (2021), <em>Atmospheric Environment</em> — <a href="https://doi.org/10.1016/j.atmosenv.2020.117834">The contribution of global aviation to anthropogenic climate forcing for 2000 to 2018</a></p>
    <p><a name="ref3">[3]</a> Google Research — <a href="https://sites.research.google/gr/contrails/">Project Contrails</a></p>
    <p><a name="ref4">[4]</a> Jaramillo, P., Kahn Ribeiro, S., Newman, P., et al. (2022), Transport. In <em>Climate Change 2022: Mitigation of Climate Change</em>, Contribution of Working Group III to the Sixth Assessment Report of the Intergovernmental Panel on Climate Change (IPCC) — <a href="https://www.ipcc.ch/report/ar6/wg3/downloads/report/IPCC_AR6_WGIII_Chapter10.pdf">Chapter 10: Transport</a> (doi:10.1017/9781009157926.012)</p>
    <p><a name="ref5">[5]</a> Teoh et al. (2024), <em>Atmospheric Chemistry and Physics</em> — <a href="https://acp.copernicus.org/articles/24/6071/2024/">Global aviation contrail climate effects from 2019 to 2021</a></p>
    <p><a name="ref6">[6]</a> Google for Developers — <a href="https://developers.google.com/contrails">Contrails API</a></p>
    <p><a name="ref7">[7]</a> ADSB.fi — <a href="https://adsb.fi/">Open flight tracking data</a></p>
    <p><a name="ref8">[8]</a> OpenSky Network — <a href="https://opensky-network.org/">The OpenSky Network</a></p>
    <p><a name="ref9">[9]</a> Bureau of Infrastructure and Transport Research Economics (BITRE) — <a href="https://www.bitre.gov.au/resource/aviation/australian-domestic-aviation-activity-monthly-publications">Australian domestic aviation activity — monthly publications</a></p>
  </div>
</details>
