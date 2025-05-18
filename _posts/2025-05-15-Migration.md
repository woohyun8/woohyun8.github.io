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

For this project, I decided to analyze the state of Florida.

![florida](/assets/images/florida.png)

**-Link**

First, I read `county-to-county-2016-2020-ins-outs-nets-gross.xlsx` and filtered the data to include only **Florida** for both O_CC and D_CC to analyze internal migration within Florida, and filtered out values with weights less than 60.

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

> SC = State Code / CC = County Code / SN = State Name / CN = County Name / Weight = Direction to Origin

```python
link = florida[(florida['O_SN'] == "Florida") &
               (florida['D_SN'] == "Florida") &
               (florida['Weight'] > 60)]
```

![florida_link](/assets/images/florida_link.png)

**-Node**

I filtered the data using the file “R13859119_SL050.csv” when `node[‘Geo_STATE’] == 12`. Then, I selected several socioeconomic and demographic indicators variables using the code and added them to the node. 

```python
node = node[['Geo_FIPS','Geo_QName','Geo_STATE','Geo_COUNTY',
             'SE_A00002_001', 'SE_A00002_002', 'SE_A14006_001',
             'SE_A12001_005', 'SE_A12001_006', 'SE_A12001_007','SE_A12001_008',
             'SE_A03001_002', 'SE_A03001_003', 'SE_A03001_005']]
```

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
  </tbody>
</table>
</div> 

**-Flordia Network**

Next, I set the source to **`O_CC` (Origin County Code)**, the target to **`D_CC` (Destination County Code)**, and assigned weights to the edge attributes to create the Florida network.

```python
g = nx.from_pandas_edgelist(link,
                            source = 'O_CC',
                            target = 'D_CC',
                            edge_attr='Weight',
                            create_using=nx.DiGraph())

degree_dict = dict(g.degree(weight = "Weight"))

nx.set_node_attributes(g, degree_dict, 'degree')
```

```python
node_unique = node.drop_duplicates(subset='Geo_COUNTY')

node_attr = node_unique.set_index('Geo_COUNTY').to_dict('index')
nx.set_node_attributes(g, node_attr)
```

**County shp file**

I filtered the data using the file “COUNTY_2019_US_SL050_Coast_Clipped.shp” when `map['STATEFP'] == 12`. Then, I stored the filtered data in a variable called “map.”

```python
merged = pd.merge(map, node, left_on='COUNTYFP', right_on='Geo_COUNTY')
```

Then, I merged the **map** and **node** dataframes to create a new dataframe called **merged**. This data will be used later to apply various algorithms to distinguish communities on the Florida map.

## 💡 Algorithms

### "Girvan-Newman"

```python
communities = list(nx.community.girvan_newman(g))
```
Next, using the above code, I calculated the community partitioning of the Florida network 'g' graph I created and stored the results in a list.

**- Calculate the Modularity**

```python
modularity_df = pd.DataFrame(
    [
        [k + 1, nx.community.modularity(g, communities[k])]
        for k in range(len(communities))
    ],
    columns=["k", "modularity"],
)
```

![modularity](/assets/images/g_n_modularity.png)

![modularity_trend](/assets/images/g_n_modu_trend.png)

The Girvan-Newman algorithm was used to calculate modularity according to the number of communities from k=1 to 66. The results showed that modularity was 0 or less in all intervals, indicating that no distinct community structure was observed. This shows that this algorithm may not be suitable for capturing the community structure of my current graph. 

Accordingly, in addition to the community-based approach, I conducted degree centrality analysis to identify the importance of individual nodes within the network.

![degree_centrality](/assets/images/degree_centrality.png)

As a result, I found that many nodes located in the center had high centrality and played a key role in the network structure. This shows that even if clear community boundaries are lacking, there are major nodes that serve as connection hubs within the network. Therefore, I applied other multi-algorithms again.

### "Louvain"

```python
g = g.to_undirected()

louv = community_louvain.best_partition(g, weight = 'Weight') # dictionary format
node['louv'] = node['Geo_COUNTY'].map(louv) # save as a column in nodes dataframe
```

Next, I applied the Louvain algorithm. This algorithm is designed to work on undirected graphs, so when using directed graphs, it is usually necessary to explicitly convert them to undirected graphs. Therefore, I added the code `g = g.to_undirected()`.

![louvain network](/assets/images/louvain_network.png)

The graph above shows the results of dividing the network into four communities using the Louvain algorithm. The node colors represent different communities, and I can see that the connections between nodes within each community are more closely formed. The modularity value is 0.14, which is low but indicates that the community structure is slightly better than that obtained using the Girvan–Newman algorithm.


### "Lieden"

```python
gg = ig.Graph.from_networkx(g)
coms = la.find_partition(gg, la.ModularityVertexPartition, weights = 'Weight')

# save it in a dictionary
for j in range(0, len(coms)):
    if j == 0:
        leid = {members : j for members in coms[j]}
    else:
        temp = {members : j for members in coms[j]}
        leid = leid | temp

# dictionary to dataframe
node['leid'] = [leid.get(node) for node in node['Geo_COUNTY']]
print(len(node['leid'].unique()))
node.head()
```

Using the above code, I applied the Leiden algorithm to detect communities in the network, extracted the community numbers to which each node belongs, and stored them in a new column `leid` in the node data frame.

### "Infomap"

```python
im = Infomap(two_level=True, silent=True, flow_model='directed', num_trials=50)
im.add_networkx_graph(g)
im.run()
info = im.get_modules(states=True) # dictionary format
node['info'] = [info.get(node) for node in node['Geo_COUNTY']] # save as a column
```

The above code applies the Infomap algorithm to detect the community structure of a directed network (g) and stores the community number to which each node belongs in a new column named ‘info’ in the node data frame.

## 🗺️ Maps

**-Louvain**

![louvain map](/assets/images/louv_map.png)

**-Lieden**

![leid map](/assets/images/leid_map.png)

**-Girvan-Newman**

![g_n map](/assets/images/g_n_map.png)

**-Infomap**

![info map](/assets/images/info_map.png)
