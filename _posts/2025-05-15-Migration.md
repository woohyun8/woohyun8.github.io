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

-   [Introduction](#introduction)
-   [Journal Summary](#journal-summary)
    -   [🧐 Research Question & Research Gap](#research-question--research-gap)
    -   [🗝️ Method](#method)
    -   [📑 Result/Interpretation](#resultinterpretation)
-   [Data Introduction](#data-introduction)
    -   [📁 Data Sources](#data-sources)
    -   [📊 Major Characteristics of the data](#major-characteristics-of-the-data)
    -   [💻 Data Pre-processing](#data-pre-processing)
-   [Analysis](#analysis)
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

In this study, five algorithms- Fast Greedy, InfoMap, Louvain, REDCAP, and WalkTrap- were used to distinguish communities. As a result, the **Louvain** method yielded the largest values of maximized modularity \( Q_{\text{max}} \).

- Here, **Modularity Q** is an indicator that quantifies how well a network is divided into communities. The *higher* the Q value, the more nodes are connected within their own communities. That is, there are fewer connections to other communities, resulting in a clear community structure. Conversely, a *low* Q value indicates that the boundaries between communities are blurred and that the network is entangled overall.


## 📑 Result/Interpretation

