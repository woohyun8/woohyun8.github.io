---
title: "Internal Migration–Florida Analysis"
layout: post
date: 2025-05-15
image: /assets/images/markdown.jpg
headerImage: false
tag:
- County-to-County
- Internal Migration
- Community Detection
- Florida
star: true
mathjax: true
category: blog
categories: R
author: rachel
description: Markdown summary with different options
---

<script type="text/javascript"
  id="MathJax-script"
  async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

-   [Introduction](#introduction)
-   [Journal Summary](#journal-summary)
    -   [🧐 Research Question & Research Gap](#research-question--research-gap)
    -   [🗝️ Method](#method)
    -   [📑 Result/Interpretation](#resultinterpretation)
-   [Data Introduction](#data-introduction)
    -   [🌐 Data Sources](#data-sources)
    -   [📈 Data Pre-processing](#data-pre-processing)
    -   [💡 Algorithms](#algorithms)
-   [Analysis](#analysis)
    -   [🔎 Examining Communities](#examining-communities)
    -   [📊 Statistical Analysis](#statistical-analysis)
-   [Conclusion](#conclusion)

# Introduction

This project was carried out as part of CSI 500 at George Mason University Korea, under the guidance of Professor Sohyun Park. I explore the **Florida Internal Migration Network** through the view of network analysis(NetworkX), with a focus on applying multiple algorithms. Furthermore, I aggregate the socioeconomic variables by community and compare the communities.

Through this project, I learned a lot about migration between communities and counties in Florida.

Above all, I want to thank Professor Park for her insightful feedback and encouragement throughout the project. Let’s dive in!

# Journal Summary

**[Andris et al. (2023)](https://epjdatascience.springeropen.com/articles/10.1140/epjds/s13688-023-00426-1)** was the background knowledge for my project, offering key insights into how internal migration flows can be modeled and interpreted through network analysis and various algorithms.

## 🧐 Research Question & Research Gap

In the early stages of the COVID-19 pandemic, most pandemic control policies in the United States were implemented at the state level. However, the actual movement patterns of people and social connections, i.e., ***functional regions***, do not fully align with state boundaries. The paper addresses this research gap by applying a community-detection algorithm to large networks of mobility and social-media connections to determine the boundaries between functional regions. 

The research question mentioned in the paper is: 
> *“Which boundaries based on five different human-network regions are able to ‘contain’ COVID-19 cases more effectively than state boundaries in the coterminous United States?”*

## 🗝️ Method

**1. Community-Detection Algorithms**

In this study, five algorithms- Fast Greedy, InfoMap, Louvain, REDCAP, and WalkTrap- were used to distinguish communities. As a result, the **Louvain** method yielded the largest values of maximized modularity ***Q_max***.

- Here, **Modularity Q** is an indicator that quantifies how well a network is divided into communities. The *higher* the Q value, the more nodes are connected within their own communities. That is, there are fewer connections to other communities, resulting in a clear community structure. Conversely, a *low* Q value indicates that the boundaries between communities are blurred and that the network is entangled overall.

**2. COVID-19 Data Connections**

Using the New York Times COVID report's weekly COVID-19 infection rate data by county, this study calculates the sum of `C (case count)`, `CR (case rate per 1,000 people)`, and `CD (case rate difference)` between counties to generate a random distribution.

**3. Statistical Analysis**

**- Permutation Test**

This test is used to determine whether the values of C, CR, and CD observed within a region are statistically significant. The boundary labels within and between regions are randomly reassigned 1,000 times to generate a distribution of expected values. This test is then performed for each of the three types of case values and for each region type. The results are compared with the actual observed values to determine significance.

**-Granger-causality & Kolmogorov-Smirnov (KS) Test**

`Grander causality` is a statistical method for verifying causal relationships between time series data based on predictability. This study analyzed whether changes in COVID-19 case rates in one region could visually predict changes in confirmed case rates in adjacent regions using lagged values of the case rates, and then *confirmed whether the inference of COVID-19 case rates in adjacent counties improved or not*. If both tests are statistically significant (p<0.001) for a pair of adjacent counties, it can be concluded that Granger causality exists, indicating the possibility of transmission between the two counties.

The `Kolmogorov–Smirnov (KS)` test measures how similarly different regional classification methods, such as commute, Twitter, and Facebook, move over time. Specifically, it calculates the maximum difference (D-statistic) between two distributions to test for significant differences.

## 📑 Result/Interpretation

The map below visualizes the boundaries of regions in the continental United States derived from each regional classification method. You can see that the number of communities and the shape of boundaries vary significantly depending on the data type.

![boundaries](/assets/images/Boundaries.png)

-> Especially, **Commute Regions** are the most dense and uniform in form, and their boundaries reflect actual commuting flows.

The second table shows the statistical significance and degree of time synchronization for each regional division. Permutation test results showed that the **Commutes and Trips networks** exhibited statistically significant differences between regions and between regions and their surroundings in both case rates and case-rate differences. Granger-causality analysis also revealed that commute regions were well connected internally, with a 46.32% connection in infection rate changes, while external connections were weaker at 30.82%. This indicates that boundaries function clearly. 

Additionally, the KS test results support this, with larger D-statistic values indicating that regional partitions better align with actual transmission patterns. In other words, **Commutes and Twitter** show statistically significant distribution differences.

![statistics](/assets/images/Statistics.png)

-> In conclusion, infection rate patterns were more similar internally in areas defined based on actual movement data, particularly for commutes, and there was a tendency for spread across regional boundaries to be suppressed. This suggests that movement-based communities may be a more effective method of responding to infectious diseases than states.


# Data Introduction

## 🌐 Data Sources

1) **[U.S. Census Bureau](https://www.census.gov/data/tables/2020/demo/geographic-mobility/county-to-county-migration-2016-2020.html)**.

Specifically: 

* **County-to-County Migration Flows** : `In-, Out-, Net, and Gross Migration`

2) **County shp file:** `COUNTY_2019_US_SL050_2019-11-13_15-15-56-579.zip`

3) **MSA shp file:** `CBSA_(MSA)_2019_US_S_2022-12-14_12-25-07-474.zip`

4) **Socioeconomic Variables Data:** `R13859119_SL050.csv`

## 📈 Data Pre-processing

<div class="side-by-side">
    <div class="toleft">
        <p>
            For this project, I decided to analyze the state of Florida.
        </p>
    </div>

    <div class="toright">
        <img class="image" src="https://woohyun8.github.io/assets/images/florida.png" alt="Florida">
    </div>
</div>

1. Make Florida Network

-**-Link**

```python
florida = state.iloc[:, [0, 1, 2, 3, 4, 5, 6, 7, 8]]

florida.columns = [
    'O_SC',
    'O_CC',
    'D_SC',
    'D_CC',
    'O_SN',
    'O_CN',
    'D_SN',
    'D_CN',
    'Weight',
]
```

```python
link = florida[(florida['O_SN'] == "Florida") &
               (florida['D_SN'] == "Florida") &
               (florida['Weight'] > 60)]
```

**-Node**









<div style="overflow-x:auto;">
 <table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Geo_FIPS</th>
      <th>Geo_QName</th>
      <th>Geo_STATE</th>
      <th>Geo_COUNTY</th>
      <th>pop</th>
      <th>pop_density</th>
      <th>income</th>
      <th>Bachelor</th>
      <th>Master</th>
      <th>Professional</th>
      <th>Doctorate</th>
      <th>White</th>
      <th>Black_African</th>
      <th>Asian</th>
      <th>g_n</th>
      <th>louv</th>
      <th>leid</th>
      <th>info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>322</th>
      <td>12001</td>
      <td>Alachua County, Florida</td>
      <td>12</td>
      <td>1</td>
      <td>281751</td>
      <td>321.763800</td>
      <td>59659.0</td>
      <td>29.084901</td>
      <td>15.098793</td>
      <td>6.631032</td>
      <td>3.686234</td>
      <td>62.683007</td>
      <td>19.356453</td>
      <td>5.887468</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>323</th>
      <td>12003</td>
      <td>Baker County, Florida</td>
      <td>12</td>
      <td>3</td>
      <td>28186</td>
      <td>48.162250</td>
      <td>70833.0</td>
      <td>10.327822</td>
      <td>4.150997</td>
      <td>0.975662</td>
      <td>0.305116</td>
      <td>78.187753</td>
      <td>11.665366</td>
      <td>0.816008</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>324</th>
      <td>12005</td>
      <td>Bay County, Florida</td>
      <td>12</td>
      <td>5</td>
      <td>181368</td>
      <td>239.083000</td>
      <td>70188.0</td>
      <td>20.066384</td>
      <td>7.303383</td>
      <td>1.781461</td>
      <td>0.714018</td>
      <td>76.512946</td>
      <td>9.998456</td>
      <td>1.952935</td>
      <td>0</td>
      <td>1</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>325</th>
      <td>12007</td>
      <td>Bradford County, Florida</td>
      <td>12</td>
      <td>7</td>
      <td>27888</td>
      <td>94.868100</td>
      <td>59740.0</td>
      <td>10.882817</td>
      <td>4.001721</td>
      <td>1.337493</td>
      <td>0.548623</td>
      <td>73.909925</td>
      <td>18.581469</td>
      <td>0.939472</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>326</th>
      <td>12009</td>
      <td>Brevard County, Florida</td>
      <td>12</td>
      <td>9</td>
      <td>620533</td>
      <td>611.380700</td>
      <td>75817.0</td>
      <td>25.094878</td>
      <td>9.926466</td>
      <td>2.297380</td>
      <td>1.025731</td>
      <td>75.371173</td>
      <td>9.804958</td>
      <td>2.420016</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>327</th>
      <td>12011</td>
      <td>Broward County, Florida</td>
      <td>12</td>
      <td>11</td>
      <td>1946127</td>
      <td>1617.850000</td>
      <td>74534.0</td>
      <td>25.624998</td>
      <td>9.839748</td>
      <td>3.127134</td>
      <td>0.974140</td>
      <td>42.635501</td>
      <td>28.250931</td>
      <td>3.712553</td>
      <td>0</td>
      <td>2</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>328</th>
      <td>12013</td>
      <td>Calhoun County, Florida</td>
      <td>12</td>
      <td>13</td>
      <td>13593</td>
      <td>23.959330</td>
      <td>46901.0</td>
      <td>9.585816</td>
      <td>2.883837</td>
      <td>0.809240</td>
      <td>0.478187</td>
      <td>76.598249</td>
      <td>11.608916</td>
      <td>0.743030</td>
      <td>3</td>
      <td>1</td>
      <td>0.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>329</th>
      <td>12015</td>
      <td>Charlotte County, Florida</td>
      <td>12</td>
      <td>15</td>
      <td>195083</td>
      <td>286.416000</td>
      <td>66154.0</td>
      <td>21.067956</td>
      <td>8.126797</td>
      <td>2.345668</td>
      <td>0.812987</td>
      <td>84.669602</td>
      <td>5.285955</td>
      <td>1.305086</td>
      <td>0</td>
      <td>0</td>
      <td>3.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>330</th>
      <td>12017</td>
      <td>Citrus County, Florida</td>
      <td>12</td>
      <td>17</td>
      <td>158693</td>
      <td>272.709400</td>
      <td>55355.0</td>
      <td>16.296875</td>
      <td>6.174185</td>
      <td>1.832469</td>
      <td>0.633298</td>
      <td>88.780854</td>
      <td>2.234503</td>
      <td>1.366790</td>
      <td>0</td>
      <td>4</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>331</th>
      <td>12019</td>
      <td>Clay County, Florida</td>
      <td>12</td>
      <td>19</td>
      <td>223436</td>
      <td>369.561400</td>
      <td>86094.0</td>
      <td>19.909057</td>
      <td>6.790311</td>
      <td>1.817970</td>
      <td>0.615389</td>
      <td>71.907839</td>
      <td>11.384468</td>
      <td>2.952971</td>
      <td>0</td>
      <td>1</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>332</th>
      <td>12021</td>
      <td>Collier County, Florida</td>
      <td>12</td>
      <td>21</td>
      <td>387681</td>
      <td>194.146500</td>
      <td>86173.0</td>
      <td>30.572816</td>
      <td>12.678465</td>
      <td>4.152383</td>
      <td>1.312161</td>
      <td>69.472324</td>
      <td>6.587633</td>
      <td>1.491432</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>333</th>
      <td>12023</td>
      <td>Columbia County, Florida</td>
      <td>12</td>
      <td>23</td>
      <td>70755</td>
      <td>88.734680</td>
      <td>55070.0</td>
      <td>11.399901</td>
      <td>3.558759</td>
      <td>0.727864</td>
      <td>0.353332</td>
      <td>72.820295</td>
      <td>16.476574</td>
      <td>0.932796</td>
      <td>0</td>
      <td>1</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>334</th>
      <td>12027</td>
      <td>DeSoto County, Florida</td>
      <td>12</td>
      <td>27</td>
      <td>34719</td>
      <td>54.530050</td>
      <td>50868.0</td>
      <td>8.093551</td>
      <td>2.615283</td>
      <td>0.472364</td>
      <td>0.187217</td>
      <td>67.692042</td>
      <td>12.854633</td>
      <td>0.034563</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>335</th>
      <td>12029</td>
      <td>Dixie County, Florida</td>
      <td>12</td>
      <td>29</td>
      <td>16952</td>
      <td>24.044210</td>
      <td>47655.0</td>
      <td>6.117272</td>
      <td>2.229825</td>
      <td>0.401133</td>
      <td>0.135677</td>
      <td>85.105002</td>
      <td>6.600991</td>
      <td>0.430628</td>
      <td>0</td>
      <td>1</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>336</th>
      <td>12031</td>
      <td>Duval County, Florida</td>
      <td>12</td>
      <td>31</td>
      <td>1007189</td>
      <td>1320.701000</td>
      <td>68447.0</td>
      <td>22.753922</td>
      <td>7.461559</td>
      <td>2.062076</td>
      <td>0.800446</td>
      <td>52.768646</td>
      <td>28.924263</td>
      <td>4.800688</td>
      <td>0</td>
      <td>1</td>
      <td>3.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>337</th>
      <td>12033</td>
      <td>Escambia County, Florida</td>
      <td>12</td>
      <td>33</td>
      <td>323275</td>
      <td>492.051700</td>
      <td>65715.0</td>
      <td>19.717269</td>
      <td>7.153971</td>
      <td>2.096976</td>
      <td>0.985848</td>
      <td>64.364705</td>
      <td>21.150414</td>
      <td>2.880210</td>
      <td>0</td>
      <td>1</td>
      <td>3.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>338</th>
      <td>12035</td>
      <td>Flagler County, Florida</td>
      <td>12</td>
      <td>35</td>
      <td>121710</td>
      <td>250.333600</td>
      <td>72923.0</td>
      <td>23.606113</td>
      <td>8.060965</td>
      <td>2.260291</td>
      <td>0.787117</td>
      <td>77.197437</td>
      <td>9.382959</td>
      <td>2.321091</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>339</th>
      <td>12037</td>
      <td>Franklin County, Florida</td>
      <td>12</td>
      <td>37</td>
      <td>12418</td>
      <td>22.785380</td>
      <td>62734.0</td>
      <td>20.180383</td>
      <td>9.027219</td>
      <td>2.029312</td>
      <td>0.740860</td>
      <td>79.996779</td>
      <td>10.911580</td>
      <td>0.314060</td>
      <td>2</td>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>340</th>
      <td>12039</td>
      <td>Gadsden County, Florida</td>
      <td>12</td>
      <td>39</td>
      <td>43642</td>
      <td>84.516080</td>
      <td>46047.0</td>
      <td>14.078182</td>
      <td>4.974566</td>
      <td>0.969250</td>
      <td>0.643875</td>
      <td>33.855002</td>
      <td>55.057055</td>
      <td>0.135191</td>
      <td>0</td>
      <td>0</td>
      <td>2.0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>341</th>
      <td>12041</td>
      <td>Gilchrist County, Florida</td>
      <td>12</td>
      <td>41</td>
      <td>18494</td>
      <td>52.886100</td>
      <td>61070.0</td>
      <td>10.322267</td>
      <td>4.055369</td>
      <td>0.946253</td>
      <td>0.259544</td>
      <td>89.088353</td>
      <td>3.201038</td>
      <td>0.227101</td>
      <td>0</td>
      <td>1</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>342</th>
      <td>12043</td>
      <td>Glades County, Florida</td>
      <td>12</td>
      <td>43</td>
      <td>12324</td>
      <td>15.274740</td>
      <td>38905.0</td>
      <td>9.891269</td>
      <td>2.198961</td>
      <td>0.559883</td>
      <td>0.259656</td>
      <td>64.321649</td>
      <td>15.108731</td>
      <td>0.348913</td>
      <td>8</td>
      <td>0</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>343</th>
      <td>12045</td>
      <td>Gulf County, Florida</td>
      <td>12</td>
      <td>45</td>
      <td>14772</td>
      <td>26.690240</td>
      <td>67361.0</td>
      <td>19.401571</td>
      <td>7.168968</td>
      <td>1.753317</td>
      <td>0.162470</td>
      <td>79.068508</td>
      <td>14.046845</td>
      <td>0.453561</td>
      <td>0</td>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>344</th>
      <td>12047</td>
      <td>Hamilton County, Florida</td>
      <td>12</td>
      <td>47</td>
      <td>13445</td>
      <td>26.142570</td>
      <td>47696.0</td>
      <td>8.107103</td>
      <td>2.394942</td>
      <td>0.401636</td>
      <td>0.297508</td>
      <td>58.676088</td>
      <td>32.182968</td>
      <td>0.490889</td>
      <td>5</td>
      <td>1</td>
      <td>1.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>345</th>
      <td>12049</td>
      <td>Hardee County, Florida</td>
      <td>12</td>
      <td>49</td>
      <td>25508</td>
      <td>40.008190</td>
      <td>54231.0</td>
      <td>8.040615</td>
      <td>2.473734</td>
      <td>0.419476</td>
      <td>0.000000</td>
      <td>61.408186</td>
      <td>6.531284</td>
      <td>1.105536</td>
      <td>6</td>
      <td>0</td>
      <td>0.0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>346</th>
      <td>12051</td>
      <td>Hendry County, Florida</td>
      <td>12</td>
      <td>51</td>
      <td>40798</td>
      <td>35.293910</td>
      <td>53044.0</td>
      <td>6.892495</td>
      <td>1.600569</td>
      <td>0.634835</td>
      <td>0.205892</td>
      <td>55.228197</td>
      <td>10.770136</td>
      <td>1.051522</td>
      <td>0</td>
      <td>0</td>
      <td>4.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>347</th>
      <td>12053</td>
      <td>Hernando County, Florida</td>
      <td>12</td>
      <td>53</td>
      <td>201512</td>
      <td>426.060600</td>
      <td>63193.0</td>
      <td>15.302811</td>
      <td>5.015086</td>
      <td>1.327464</td>
      <td>0.381119</td>
      <td>78.708960</td>
      <td>5.001191</td>
      <td>1.379074</td>
      <td>0</td>
      <td>4</td>
      <td>4.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>348</th>
      <td>12055</td>
      <td>Highlands County, Florida</td>
      <td>12</td>
      <td>55</td>
      <td>103808</td>
      <td>102.006600</td>
      <td>55581.0</td>
      <td>15.297472</td>
      <td>5.647927</td>
      <td>2.125077</td>
      <td>0.639642</td>
      <td>72.609047</td>
      <td>9.404863</td>
      <td>1.501811</td>
      <td>0</td>
      <td>4</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>349</th>
      <td>12057</td>
      <td>Hillsborough County, Florida</td>
      <td>12</td>
      <td>57</td>
      <td>1489634</td>
      <td>1456.852000</td>
      <td>75011.0</td>
      <td>25.680268</td>
      <td>9.684594</td>
      <td>2.795519</td>
      <td>1.041531</td>
      <td>54.813464</td>
      <td>16.403157</td>
      <td>4.424577</td>
      <td>0</td>
      <td>4</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>350</th>
      <td>12059</td>
      <td>Holmes County, Florida</td>
      <td>12</td>
      <td>59</td>
      <td>19626</td>
      <td>40.985130</td>
      <td>48236.0</td>
      <td>8.738408</td>
      <td>2.715785</td>
      <td>0.825436</td>
      <td>0.382146</td>
      <td>84.836441</td>
      <td>6.674819</td>
      <td>0.453480</td>
      <td>7</td>
      <td>1</td>
      <td>0.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>351</th>
      <td>12061</td>
      <td>Indian River County, Florida</td>
      <td>12</td>
      <td>61</td>
      <td>163856</td>
      <td>326.063800</td>
      <td>71049.0</td>
      <td>26.212040</td>
      <td>10.386559</td>
      <td>2.814667</td>
      <td>0.992335</td>
      <td>77.273948</td>
      <td>8.062567</td>
      <td>1.495826</td>
      <td>0</td>
      <td>4</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>352</th>
      <td>12063</td>
      <td>Jackson County, Florida</td>
      <td>12</td>
      <td>63</td>
      <td>47652</td>
      <td>51.895480</td>
      <td>47327.0</td>
      <td>10.721481</td>
      <td>4.576933</td>
      <td>0.980022</td>
      <td>0.495257</td>
      <td>66.620499</td>
      <td>25.407118</td>
      <td>0.602283</td>
      <td>0</td>
      <td>1</td>
      <td>1.0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>353</th>
      <td>12065</td>
      <td>Jefferson County, Florida</td>
      <td>12</td>
      <td>65</td>
      <td>14713</td>
      <td>24.600410</td>
      <td>56984.0</td>
      <td>14.035207</td>
      <td>5.260654</td>
      <td>1.882689</td>
      <td>0.700061</td>
      <td>62.788011</td>
      <td>30.136614</td>
      <td>0.700061</td>
      <td>9</td>
      <td>0</td>
      <td>1.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>354</th>
      <td>12067</td>
      <td>Lafayette County, Florida</td>
      <td>12</td>
      <td>67</td>
      <td>8035</td>
      <td>14.788030</td>
      <td>60692.0</td>
      <td>5.774736</td>
      <td>2.389546</td>
      <td>0.634723</td>
      <td>0.510268</td>
      <td>76.776602</td>
      <td>17.660236</td>
      <td>0.024891</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>355</th>
      <td>12069</td>
      <td>Lake County, Florida</td>
      <td>12</td>
      <td>69</td>
      <td>398696</td>
      <td>418.741100</td>
      <td>69956.0</td>
      <td>19.928216</td>
      <td>7.134759</td>
      <td>1.562343</td>
      <td>0.716335</td>
      <td>70.619219</td>
      <td>10.116480</td>
      <td>2.099595</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>356</th>
      <td>12071</td>
      <td>Lee County, Florida</td>
      <td>12</td>
      <td>71</td>
      <td>792692</td>
      <td>1014.936000</td>
      <td>73099.0</td>
      <td>23.281047</td>
      <td>9.041721</td>
      <td>2.819128</td>
      <td>1.053751</td>
      <td>70.453846</td>
      <td>7.963370</td>
      <td>1.689811</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>357</th>
      <td>12073</td>
      <td>Leon County, Florida</td>
      <td>12</td>
      <td>73</td>
      <td>295335</td>
      <td>441.835300</td>
      <td>65074.0</td>
      <td>29.140806</td>
      <td>12.609748</td>
      <td>4.435302</td>
      <td>2.120304</td>
      <td>56.496521</td>
      <td>30.670933</td>
      <td>3.448626</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>358</th>
      <td>12075</td>
      <td>Levy County, Florida</td>
      <td>12</td>
      <td>75</td>
      <td>44276</td>
      <td>39.593870</td>
      <td>53805.0</td>
      <td>11.934231</td>
      <td>3.665643</td>
      <td>0.693378</td>
      <td>0.212305</td>
      <td>82.229650</td>
      <td>8.367965</td>
      <td>0.883097</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>359</th>
      <td>12077</td>
      <td>Liberty County, Florida</td>
      <td>12</td>
      <td>77</td>
      <td>7650</td>
      <td>9.155499</td>
      <td>53824.0</td>
      <td>11.333333</td>
      <td>5.189542</td>
      <td>0.666667</td>
      <td>0.300654</td>
      <td>77.307190</td>
      <td>13.124183</td>
      <td>0.209150</td>
      <td>4</td>
      <td>1</td>
      <td>NaN</td>
      <td>4</td>
    </tr>
    <tr>
      <th>360</th>
      <td>12079</td>
      <td>Madison County, Florida</td>
      <td>12</td>
      <td>79</td>
      <td>18113</td>
      <td>26.004660</td>
      <td>48176.0</td>
      <td>9.413129</td>
      <td>3.323580</td>
      <td>0.822614</td>
      <td>0.458234</td>
      <td>56.097830</td>
      <td>36.460001</td>
      <td>0.138022</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>2</td>
    </tr>
    <tr>
      <th>361</th>
      <td>12081</td>
      <td>Manatee County, Florida</td>
      <td>12</td>
      <td>81</td>
      <td>416020</td>
      <td>560.098900</td>
      <td>75792.0</td>
      <td>26.043700</td>
      <td>10.049036</td>
      <td>3.035191</td>
      <td>1.199942</td>
      <td>74.602663</td>
      <td>7.925100</td>
      <td>2.204221</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>362</th>
      <td>12083</td>
      <td>Marion County, Florida</td>
      <td>12</td>
      <td>83</td>
      <td>387697</td>
      <td>244.076800</td>
      <td>58535.0</td>
      <td>16.708151</td>
      <td>6.051633</td>
      <td>1.668571</td>
      <td>0.547851</td>
      <td>71.741592</td>
      <td>12.582249</td>
      <td>1.576747</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>363</th>
      <td>12085</td>
      <td>Martin County, Florida</td>
      <td>12</td>
      <td>85</td>
      <td>160464</td>
      <td>295.066900</td>
      <td>80701.0</td>
      <td>28.461836</td>
      <td>10.163650</td>
      <td>3.231254</td>
      <td>0.968442</td>
      <td>80.892287</td>
      <td>5.029789</td>
      <td>1.415271</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>364</th>
      <td>12086</td>
      <td>Miami-Dade County, Florida</td>
      <td>12</td>
      <td>86</td>
      <td>2685296</td>
      <td>1413.370000</td>
      <td>68694.0</td>
      <td>23.807320</td>
      <td>9.154410</td>
      <td>3.367338</td>
      <td>0.929432</td>
      <td>36.903976</td>
      <td>15.445597</td>
      <td>1.561727</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>365</th>
      <td>12087</td>
      <td>Monroe County, Florida</td>
      <td>12</td>
      <td>87</td>
      <td>81840</td>
      <td>83.252490</td>
      <td>82430.0</td>
      <td>29.791056</td>
      <td>11.933040</td>
      <td>3.591153</td>
      <td>1.212121</td>
      <td>72.568426</td>
      <td>7.416911</td>
      <td>1.171799</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>366</th>
      <td>12089</td>
      <td>Nassau County, Florida</td>
      <td>12</td>
      <td>89</td>
      <td>94653</td>
      <td>145.916100</td>
      <td>88900.0</td>
      <td>25.668494</td>
      <td>9.771481</td>
      <td>2.149958</td>
      <td>0.971971</td>
      <td>85.920150</td>
      <td>5.546575</td>
      <td>1.280467</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>367</th>
      <td>12091</td>
      <td>Okaloosa County, Florida</td>
      <td>12</td>
      <td>91</td>
      <td>214281</td>
      <td>230.443900</td>
      <td>79097.0</td>
      <td>23.583986</td>
      <td>8.976064</td>
      <td>1.918976</td>
      <td>0.777017</td>
      <td>72.941605</td>
      <td>9.350806</td>
      <td>3.332073</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>368</th>
      <td>12093</td>
      <td>Okeechobee County, Florida</td>
      <td>12</td>
      <td>93</td>
      <td>40249</td>
      <td>52.319920</td>
      <td>52288.0</td>
      <td>11.466123</td>
      <td>3.341698</td>
      <td>0.566474</td>
      <td>0.029814</td>
      <td>72.647768</td>
      <td>8.104549</td>
      <td>0.941638</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>369</th>
      <td>12095</td>
      <td>Orange County, Florida</td>
      <td>12</td>
      <td>95</td>
      <td>1440471</td>
      <td>1595.220000</td>
      <td>77011.0</td>
      <td>26.166650</td>
      <td>9.447188</td>
      <td>2.647537</td>
      <td>0.934694</td>
      <td>46.624680</td>
      <td>20.313911</td>
      <td>5.343599</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>370</th>
      <td>12097</td>
      <td>Osceola County, Florida</td>
      <td>12</td>
      <td>97</td>
      <td>406943</td>
      <td>306.538700</td>
      <td>68711.0</td>
      <td>18.946634</td>
      <td>5.820717</td>
      <td>1.295267</td>
      <td>0.490486</td>
      <td>43.416154</td>
      <td>11.005964</td>
      <td>2.901881</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>371</th>
      <td>12099</td>
      <td>Palm Beach County, Florida</td>
      <td>12</td>
      <td>99</td>
      <td>1507453</td>
      <td>767.428300</td>
      <td>81115.0</td>
      <td>29.165486</td>
      <td>11.479628</td>
      <td>3.950637</td>
      <td>1.225909</td>
      <td>57.379832</td>
      <td>18.482898</td>
      <td>2.836175</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>372</th>
      <td>12101</td>
      <td>Pasco County, Florida</td>
      <td>12</td>
      <td>101</td>
      <td>588758</td>
      <td>788.581800</td>
      <td>67384.0</td>
      <td>20.329405</td>
      <td>7.210603</td>
      <td>1.788341</td>
      <td>0.822070</td>
      <td>75.084840</td>
      <td>6.149895</td>
      <td>3.095499</td>
      <td>0</td>
      <td>4</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>373</th>
      <td>12103</td>
      <td>Pinellas County, Florida</td>
      <td>12</td>
      <td>103</td>
      <td>960565</td>
      <td>3508.505000</td>
      <td>70293.0</td>
      <td>27.943762</td>
      <td>10.136534</td>
      <td>3.064238</td>
      <td>1.086027</td>
      <td>75.066133</td>
      <td>9.714179</td>
      <td>3.617038</td>
      <td>0</td>
      <td>4</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>374</th>
      <td>12105</td>
      <td>Polk County, Florida</td>
      <td>12</td>
      <td>105</td>
      <td>760961</td>
      <td>423.198400</td>
      <td>63644.0</td>
      <td>15.693840</td>
      <td>5.083309</td>
      <td>1.271550</td>
      <td>0.548386</td>
      <td>59.843540</td>
      <td>14.777367</td>
      <td>1.770787</td>
      <td>0</td>
      <td>4</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>375</th>
      <td>12107</td>
      <td>Putnam County, Florida</td>
      <td>12</td>
      <td>107</td>
      <td>74235</td>
      <td>101.921600</td>
      <td>47256.0</td>
      <td>10.080151</td>
      <td>2.960868</td>
      <td>0.665454</td>
      <td>0.377181</td>
      <td>73.046407</td>
      <td>15.285243</td>
      <td>0.603489</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>376</th>
      <td>12109</td>
      <td>St. Johns County, Florida</td>
      <td>12</td>
      <td>109</td>
      <td>292243</td>
      <td>486.551900</td>
      <td>106169.0</td>
      <td>34.792621</td>
      <td>13.195526</td>
      <td>3.641490</td>
      <td>1.467272</td>
      <td>81.166700</td>
      <td>5.230236</td>
      <td>3.399568</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>377</th>
      <td>12111</td>
      <td>St. Lucie County, Florida</td>
      <td>12</td>
      <td>111</td>
      <td>346237</td>
      <td>605.443500</td>
      <td>69027.0</td>
      <td>18.590445</td>
      <td>6.599815</td>
      <td>1.855088</td>
      <td>0.838443</td>
      <td>59.768598</td>
      <td>20.976961</td>
      <td>1.960507</td>
      <td>0</td>
      <td>2</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>378</th>
      <td>12113</td>
      <td>Santa Rosa County, Florida</td>
      <td>12</td>
      <td>113</td>
      <td>193719</td>
      <td>191.346100</td>
      <td>88968.0</td>
      <td>21.853303</td>
      <td>7.895973</td>
      <td>1.717952</td>
      <td>0.800644</td>
      <td>81.267713</td>
      <td>5.538435</td>
      <td>2.150021</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>379</th>
      <td>12115</td>
      <td>Sarasota County, Florida</td>
      <td>12</td>
      <td>115</td>
      <td>449011</td>
      <td>807.564600</td>
      <td>80633.0</td>
      <td>31.860689</td>
      <td>13.557574</td>
      <td>3.999902</td>
      <td>1.527802</td>
      <td>84.008410</td>
      <td>4.109031</td>
      <td>1.870556</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>380</th>
      <td>12117</td>
      <td>Seminole County, Florida</td>
      <td>12</td>
      <td>117</td>
      <td>474912</td>
      <td>1535.050000</td>
      <td>83030.0</td>
      <td>30.191488</td>
      <td>10.406349</td>
      <td>2.903696</td>
      <td>1.169480</td>
      <td>62.666978</td>
      <td>11.928947</td>
      <td>5.175906</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>381</th>
      <td>12119</td>
      <td>Sumter County, Florida</td>
      <td>12</td>
      <td>119</td>
      <td>137536</td>
      <td>246.859000</td>
      <td>73297.0</td>
      <td>31.567735</td>
      <td>12.390938</td>
      <td>2.902513</td>
      <td>1.044090</td>
      <td>86.300314</td>
      <td>6.838210</td>
      <td>1.015007</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>2</td>
    </tr>
    <tr>
      <th>382</th>
      <td>12121</td>
      <td>Suwannee County, Florida</td>
      <td>12</td>
      <td>121</td>
      <td>44484</td>
      <td>64.601930</td>
      <td>55479.0</td>
      <td>11.606420</td>
      <td>4.282439</td>
      <td>0.854240</td>
      <td>0.314720</td>
      <td>76.980487</td>
      <td>9.960885</td>
      <td>0.712616</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>2</td>
    </tr>
    <tr>
      <th>383</th>
      <td>12123</td>
      <td>Taylor County, Florida</td>
      <td>12</td>
      <td>123</td>
      <td>21422</td>
      <td>20.532200</td>
      <td>44985.0</td>
      <td>11.002707</td>
      <td>3.141630</td>
      <td>0.639529</td>
      <td>0.256745</td>
      <td>73.326487</td>
      <td>20.054150</td>
      <td>0.275418</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
    <tr>
      <th>384</th>
      <td>12125</td>
      <td>Union County, Florida</td>
      <td>12</td>
      <td>125</td>
      <td>15551</td>
      <td>63.850050</td>
      <td>64922.0</td>
      <td>7.195679</td>
      <td>2.520738</td>
      <td>0.450132</td>
      <td>0.231496</td>
      <td>73.230017</td>
      <td>16.191885</td>
      <td>0.199344</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>385</th>
      <td>12127</td>
      <td>Volusia County, Florida</td>
      <td>12</td>
      <td>127</td>
      <td>568229</td>
      <td>516.048300</td>
      <td>66581.0</td>
      <td>20.262957</td>
      <td>7.040119</td>
      <td>2.105665</td>
      <td>0.863032</td>
      <td>72.630225</td>
      <td>10.675450</td>
      <td>1.864213</td>
      <td>0</td>
      <td>3</td>
      <td>NaN</td>
      <td>1</td>
    </tr>
    <tr>
      <th>386</th>
      <td>12129</td>
      <td>Wakulla County, Florida</td>
      <td>12</td>
      <td>129</td>
      <td>34608</td>
      <td>57.069530</td>
      <td>74183.0</td>
      <td>15.444406</td>
      <td>5.547850</td>
      <td>1.441863</td>
      <td>0.641470</td>
      <td>78.033981</td>
      <td>13.866736</td>
      <td>0.676144</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>4</td>
    </tr>
    <tr>
      <th>387</th>
      <td>12131</td>
      <td>Walton County, Florida</td>
      <td>12</td>
      <td>131</td>
      <td>79846</td>
      <td>76.900860</td>
      <td>79281.0</td>
      <td>25.072014</td>
      <td>8.500113</td>
      <td>2.611277</td>
      <td>0.827844</td>
      <td>84.175788</td>
      <td>3.844901</td>
      <td>1.120908</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
    <tr>
      <th>388</th>
      <td>12133</td>
      <td>Washington County, Florida</td>
      <td>12</td>
      <td>133</td>
      <td>25259</td>
      <td>43.201640</td>
      <td>52723.0</td>
      <td>9.172968</td>
      <td>3.974821</td>
      <td>1.041213</td>
      <td>0.265252</td>
      <td>78.027634</td>
      <td>13.571400</td>
      <td>0.894731</td>
      <td>0</td>
      <td>1</td>
      <td>NaN</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
