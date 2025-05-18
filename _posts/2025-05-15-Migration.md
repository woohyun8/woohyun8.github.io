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
    -   [🗺️ Maps](#maps)
    -   [✔️ Modularity Check](#modularity-check)
-   [Analysis](#analysis)
    -   [🔎 Metropolitan Statistical Areas](#metropolitan-statistical-areas)
    -   [👥 Examine Communities](#examine-communities)
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

- As a result of applying the Louvain algorithm, Florida was divided into five major communities in a balanced manner, showing a pattern that was relatively consistent with the regional structure. The southern coastal region was separated into an independent community (2), which can be said to be connected to the characteristics of the high-density metropolitan network in that region. The northern region was divided into two communities (0,1), partially reflecting geographical boundaries. This indicates that the Louvain algorithm's high modularity effectively separated these regional characteristics.

**-Leiden**

![leid map](/assets/images/leid_map.png)

- The Leiden algorithm divided Florida into five communities, with independent communities (3 and 4) forming in the southwest coast and southern inland regions, revealing regional differentiation based on urban density and connection density.

- Meanwhile, many counties are marked as N/A, indicating that the algorithm did not classify areas with connection structures below a certain threshold as communities. Overall, the results appear to effectively reflect the differences in connection density between urban and non-urban areas.

**-Girvan-Newman**

![g_n map](/assets/images/g_n_map.png)

- The Girvan–Newman algorithm divides networks based on edge betweenness. Looking at the map above, I can see that most counties belong to community 0, with only a few counties scattered across other communities, indicating that the overall structure is not modularized.

- In particular, the northwestern and rural regions, such as Hardee and Glades, are separated into relatively independent small communities. Overall, most counties are grouped into a single community, and structural boundaries are not clearly defined. These results align with the low modularity values observed when applying the Girvan–Newman algorithm.

**-Infomap**

![info map](/assets/images/info_map.png)

- The Infomap algorithm shows that most counties across Florida belong to a single large community (No. 1), indicating a centralized network structure in which information flows from a few central locations and spreads nationwide. In the northern and some central inland regions, smaller communities are fragmented, likely indicating rural areas with weak connections to metropolitan areas or independent flow patterns. In this algorithm, the separation of parts of the Florida Panhandle (northwest) and the central southern inland regions is a notable feature.


![all map](/assets/images/all_map.png)

- Each algorithm defines communities based on different criteria, resulting in varying outcomes. In particular, Louvain and Leiden relatively well reflect the regional structure, while Girvan–Newman exhibited low partition quality and Infomap demonstrated a unified structure centered on information flow.

- This indicates that the urban-centric structure and differences in inter-regional connectivity density inherent to the Florida region significantly influenced the analysis results.


## ✔️ Modularity Check

![modularity](/assets/images/modularity_df.png)

- The table above compares the modularity values of network community detection algorithms. The **Leiden (0.3020)** and **Louvain (0.3019)** algorithms show the highest modularity and are evaluated as forming relatively well-separated community structures.
  
- On the other hand, **Girvan–Newman (0.0015)** and **Infomap (0.0191)** have very low modularity, indicating that the segmentation does not adequately reflect the structural boundaries within the network. In particular, Girvan-Newman resulted in an excessive number of nodes being grouped into a single community.

![modularity_graph](/assets/images/modularity_graph.png)

- The bar graph above visualizes the modularity values of each community detection algorithm. **Louvain** and **Leiden** showed structurally stable community partitions with high modularity, while Girvan–Newman and Infomap showed relatively poor results. Accordingly, I compared and analyzed the characteristics of each community by applying socioeconomic and demographic indicators based on the Louvain algorithm.

# Analysis

## 🔎 Metropolitan Statistical Areas

Read the file “CBSA__MSA__2019_US_SL310_Coast_Clipped.shp” into a variable called msa, and filter only the Metro Areas in Florida using the code `msa[‘NAMELSAD’].str.contains(‘Metro’)` and `msa_florida[‘NAME’].str.contains(“FL”)`.

```python
fig, ax = plt.subplots(1, 1)
fig.set_size_inches(10, 10)

msa_florida.plot(ax=ax, facecolor='none', edgecolor='black')

label_offsets = {
    'Pensacola-Ferry Pass-Brent, FL': (0.6, 0.2),
    'Crestview-Fort Walton Beach-Destin, FL': (0.6, 0.4),
    'Naples-Marco Island, FL': (0.6, -0.5),
    'Miami-Fort Lauderdale-Pompano Beach, FL': (0.6, -0.7),
    'Tallahassee, FL': (0.4, 0.2),
    'Panama City, FL': (0.3, 0.2),
    'Cape Coral-Fort Myers, FL': (0.4, -0.3),
}

for idx, row in msa_florida.iterrows():
    centroid = row['geometry'].centroid
    offset = label_offsets.get(row['NAME'], (0.5, 0.2))  

    ax.annotate(
        row['NAME'],
        xy=(centroid.x, centroid.y),
        xytext=(centroid.x + offset[0], centroid.y + offset[1]),
        fontsize=6.5,
        arrowprops=dict(arrowstyle='-', lw=0.5),
        bbox=dict(boxstyle="round,pad=0.2", facecolor="white", edgecolor="gray", alpha=0.8)
    )

plt.title("Florida Metropolitan Statistical Areas", fontsize=14)
plt.axis("off")
plt.tight_layout()
plt.show()
```

![fl_msa](/assets/images/fl_msa.png)

Next, I merged the **Louvain algorithm map** created above with the **MSA boundaries**.

![louv_msa](/assets/images/louv_msa.png)

- **Community 0**: Includes **southern Florida** -> Features a strongly connected network of large cities and a consistent network structure around them.

- **Community 1**: Includes most of the **northern and Panhandle regions** -> Has a wide geographic range but exhibits similar connection patterns. For example, interconnections between rural or low-density areas can be observed.

- **Community 2**: Includes some **metropolitan areas or suburban regions** such as Miami and Port -> Independent communities with low connectivity to other MSAs and high internal connectivity.

- **Community 3**: Includes parts of **central inland Florida and the eastern coast**, such as Orlando and Palm Bay -> Multiple adjacent metropolitan areas are grouped into a single community, indicating high interaction between counties.

- **Community 4**: Includes parts of the **central and southern regions**, such as Petersburg and Lakeland -> Low connectivity with major metropolitan areas but forms an interconnected independent network.

## 👥 Examine Communities

### Boxplot

To compare the relationship between **population and income**, I created boxplots for each.

![pop_income_box](/assets/images/pop_income_boxplot.png)

-> **Community 2** exhibits the highest population concentration, with a wide interquartile range and extreme upper outliers, suggesting the presence of highly urbanized counties such as Miami-Dade. In contrast, Communities 1 and 0 generally include counties with smaller populations.

-> Regarding income, **Community 2** also shows the highest median household income, while Community 1 has the lowest overall income levels and a wider spread. Communities 3 and 4 display relatively moderate income distributions.

![edu_box](/assets/images/edu_boxplot.png)

-> Community 2 consistently shows the highest median values for Bachelor’s, Master’s, and Professional degrees, suggesting a relatively more educated population. Communities 3 and 4 follow with moderate levels of higher education, while Community 1 exhibits the lowest educational attainment across all three categories, including a notably low median for Professional degrees. These gaps indicate that the network-based communities identified by the Louvain algorithm also reflect underlying differences in educational composition across Florida counties.

-> Notably, Community 2, which exhibited the highest median income in the earlier boxplot, also shows the highest levels of educational achievement across all degree types (Bachelor, Master, and Professional). In contrast, Community 1, which had the lowest income, likewise demonstrates the lowest overall education levels. This association indicates a clear relationship between **income level and educational standard** across the detected communities.

![race_box](/assets/images/race_boxplot.png)

-> **Community 1 and Community 4** show the highest median proportions of *White* residents, whereas Community 2 has the most racially diverse distribution, with lower White representation and higher proportions of both Black/African American and Asian populations. Community 3 demonstrates a relatively elevated Asian population, while Communities 0 and 4 have lower overall diversity, particularly in terms of Black/African American representation.

-> Community 2, which has the highest median income, includes counties with greater racial diversity, particularly higher proportions of Asian and Black/African American populations. In contrast, Community 1, with the lowest income, is predominantly White, but also shows the lowest overall diversity. These patterns show that racial demographics may partially reflect or interact with underlying socioeconomic structures within the network communities.

### Violin Plot

To further explore the distributions, I visualized the variables using violin plots.

![edu](/assets/images/edu_violin.png)

<table style="width: 100%; table-layout: fixed; border-collapse: collapse; font-family: Arial, sans-serif; font-size: 15px;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="padding: 8px; border: 1px solid #ddd;">Community</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Income (Normalized)</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Bachelor</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Master</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Professional</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Note</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>0</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Upper–middle (50~75%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Medium–High (10~35%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Medium (5~15%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (0~6%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>High income, High education</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>1</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (50~70%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (10~30%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (~10%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Very Low (0~3%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>Moderate income, Low education</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>2</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Highest (60~85%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Highest (20~35%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Highest (~15%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Highest (~7%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>High income, High education</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>3</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Upper–moderate (60~75%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (15~25%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (~10%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (~4%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>Moderate income, education</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>4</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (55~70%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (10~20%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (~10%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Lowest (0~2%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>Moderate income, Low education</b></td>
    </tr>
  </tbody>
</table>


![race](/assets/images/race_violin.png)

<table style="width: 100%; table-layout: fixed; border-collapse: collapse; font-family: Arial, sans-serif; font-size: 15px;">
  <thead>
    <tr style="background-color: #f2f2f2;">
      <th style="padding: 8px; border: 1px solid #ddd;">Community</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Income (Normalized)</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Bachelor</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Master</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Professional</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Note</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>0</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate to High (45–75%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Medium–High (60–80%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (0–25%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Very Low (0–5%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>Mixed race, modest education</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>1</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (50–70%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">High (70–85%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (0–20%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Very Low (0–4%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>White dominant, low diversity</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>2</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">High (60–85%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate to High (60–80%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (10–25%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (0–4%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>High income, racially mixed</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>3</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate to High (60–75%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">High (~75–85%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (~10–20%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Low (~0–4%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>High White, low diversity</b></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>4</b></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Moderate (55–70%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">High (~75–85%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Very Low (~10%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Very Low (~0–3%)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><b>White dominant, modest income</b></td>
    </tr>
  </tbody>
</table>

### Choropleth Maps of Population and Income

![maps](/assets/images/maps.png)

-> The figure displays two side-by-side choropleth maps of Florida counties, illustrating the log-scaled distribution of total population and median household income. Furthermore, County-level Louvain community boundaries are overlaid to highlight how these socioeconomic indicators vary across algorithmically detected communities.

## 📊 Statistical Analysis

### ANOVA Test

I conducted ANOVA tests on all socioeconomic and demographic indicators variables I selected — income, pop (population), Bachelor, Master, Professional, White, Black_African, and Asian. As a result, only the pop, Bachelor, White, and Asian variables exhibited statistically significant differences (p<0.05). In particular, the total population (`pop`) showed the lowest p-value (0.00016), indicating a clear heterogeneity in community composition.

```python
from statsmodels.formula.api import ols
from statsmodels.stats.anova import anova_lm

variables = ['pop', 'Bachelor', 'White', 'Asian']

anova_results = []

for var in variables:
    model = ols(f'{var} ~ C(louv)', data=node_data).fit()
    anova_table = anova_lm(model)
    
    row = anova_table.loc['C(louv)'].copy()
    row['Variable'] = var
    anova_results.append(row)

combined_anova = pd.DataFrame(anova_results)

combined_anova = combined_anova[['Variable', 'df', 'sum_sq', 'mean_sq', 'F', 'PR(>F)']]

display(combined_anova)
```
![anova](/assets/images/anova_test.png)

-> These results show clear differences between communities in terms of population size, percentage of residents with bachelor's degrees, percentage of white residents, and percentage of Asian residents.

### Tukey Test

Once again, I conducted a Tukey test on all socioeconomic and demographic indicator variables I selected.

```python
tukey = MultiComparison(node_data['pop'], node_data['louv'])
result = tukey.tukeyhsd()
result.summary()
```
![pop_tukey](/assets/images/pop_tukey.png)

-> The analysis results show that there is a statistically significant difference (p < 0.05) between **Community 0 vs 2** and **Community 1 vs 2**. When looking at the population boxplot created above, the difference in population between Community 2 and Communities 0 and 1 is clearly obvious. In particular, Community 2 exhibits a **wide distribution** with some counties exceeding 2.5 million in population and a high median, demonstrating a distinct population density characteristic compared to other communities. This difference was also confirmed to be statistically significant in the TukeyHSD Test conducted, indicating that Community 2 is composed primarily of Florida's metropolitan areas.

```python
tukey = MultiComparison(node_data['White'], node_data['louv'])
result = tukey.tukeyhsd()
result.summary()
```
![white_tukey](/assets/images/white_tukey.png)

-> The analysis results show that there is a statistically significant difference (p < 0.05) between **Community 1 vs 2**. Looking at the population boxplot created above, **Community 1** has the highest percentage of white people among all communities, with low dispersion and a median of over 75%. In contrast, Community 2 has the lowest percentage of white people, with high dispersion and a median of about 60%. Community 2 is likely to include many counties with more diverse ethnic compositions and relatively low percentages of white people. Conversely, Community 1 may include areas with a high proportion of white residents. In fact, there is [data](https://www.indexmundi.com/facts/united-states/quick-facts/florida/white-population-percentage?utm_source=chatgpt.com#map) indicating that Citrus County in Florida, colored in Community 1, has the highest percentage of white residents. Community 1 can be interpreted as a group of areas with a relatively *high percentage of white residents, despite its small population*.

### Mann-Whitney U Test & Kolmogorov-Smirnov Test

```python
from scipy.stats import mannwhitneyu, ks_2samp
import itertools

variables = ['pop', 'Bachelor', 'Master', 'Professional', 'White', 'Black_African', 'Asian']
groups = sorted(node_data['louv'].dropna().unique())

for var in variables:
    print(f"\n=== {var.upper()} ===")
    for g1, g2 in itertools.combinations(groups, 2):
        data1 = node_data[node_data['louv'] == g1][var].dropna()
        data2 = node_data[node_data['louv'] == g2][var].dropna()

        u_stat, u_p = mannwhitneyu(data1, data2, alternative='two-sided')
        ks_stat, ks_p = ks_2samp(data1, data2)

        print(f"{g1} vs {g2} | U p={u_p:.4f} | KS p={ks_p:.4f}")
```
![statistic](/assets/images/all_statistic.png)

The **Mann–Whitney U test** and **Kolmogorov–Smirnov (KS) test** indicate that a distribution difference is strongly significant when p < 0.05. However, if only one of the two tests is significant, it provides weak evidence that a distribution difference exists. In the figure above, I highlighted only the results seemed significant. Based on this, the following conclusions can be interpreted:

**- Population**: Community 3 has a significantly smaller or larger population compared to communities 0, 1, and 4. In particular, the difference between community 1 and 3 (p < 0.001) is very strong.

**- Bachelor's Degree**: The proportion of bachelor's degrees in Community 1 is statistically significantly different from other communities. While other communities have a high number or wide distribution, Community 1 not only has a narrow distribution but also shows low proportions across all three types of educational degrees (bachelor, master, professional). Therefore, Community 1 exhibits independent characteristics at the bachelor's degree level.

**- Master's Degree**: Similar to the explanation for bachelor's degrees, there are statistically significant differences in the proportion of master's degrees between Community 1 and Communities 2, 3, and 4. Community 1 shows a general trend toward lower levels of higher education.

**- Professional Degree**: Similar to the previous categories, the proportion of professional degree holders in Community 1 is distributed differently from other communities. Additionally, Communities 2 and 4 also show significant results, suggesting the possibility of regional disparities in professional education levels.

**- White Population**: Based on the boxplot, I can see that Community 1 is at the center and shows a significant difference in the percentage of white population compared to other communities.

**- Asian Population**: Based on the boxplot, I can see that the percentage of Asian population shows the most significant difference between Community 1 and 2, 3, and 4.

- The results of the multivariate statistical analysis conducted in this study show that **Community 1 consistently exhibits statistically significant differences from other communities** in various variables, such as population characteristics, education level, and ethnic composition. In particular, significant differences were found in the percentages of bachelor's, master's, and professional degrees, as well as the percentages of white and Asian populations. This indicates that the socioeconomic characteristics or urban structure of Community 1 clearly distinguishes it from other regions.


## Conclusion

The key findings I discovered through this project are

- Community 2 had the **highest income level** and **highest education level** among the analyzed communities. Community 2 is actually located in an area densely populated with large counties in Florida, so this result can be seen as related to the socioeconomic advantages of the region.
  
- Community 1 is characterized by **low educational attainment** and **specific racial composition**, showing distinct differences from other communities. This reflects the unique social characteristics of the region. In fact, the areas within Community 1 are among the poorest in Florida, as indicated by the [link](https://hdpulse.nimhd.nih.gov/data-portal/social/map?age=001&age_options=ageall_1&demo=00008&demo_options=poverty_3&race=00&race_options=race_7&sex=0&sex_options=sexboth_1&socialtopic=080&socialtopic_options=social_6&statefips=12&statefips_options=area_states&utm_source=chatgpt.com).
  
- Community 3 showed a significant difference in population size compared to other communities, which may be related to the population density or urban structure of the area.

Through this Florida internal migration analysis, I gained insights into the socioeconomic characteristics and population composition of each community, providing valuable information for policy decisions, urban planning, and the provision of social services. Future research will focus on analyzing changes over time, integrating additional variables, and conducting more detailed evaluations of policy impacts. 

Thank you for taking the time to read my lengthy project analysis!

Feel free to leave a comment and share your thoughts!😊
