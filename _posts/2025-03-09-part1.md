-   [Introduction](#introduction)
-   [Global Malaria Overview](#global-malaria-overview)
-   [1. Analysis of Malaria Incidence Cases in Neighboring
    Countries](#analysis-of-malaria-incidence-cases-in-neighboring-countries)
    -   [Why we select the South Korea?](#why-we-select-the-south-korea)
-   [2. Number of malaria infections by ‘Seoul’, ‘Gyeonggi’, ‘Gangwon’,
    ‘Incheon’(2001-2023)](#number-of-malaria-infections-by-seoul-gyeonggi-gangwon-incheon2001-2023)
    -   [Overall Trends](#overall-trends)
    -   [Regional Trends](#regional-trends)
-   [Malaria Key Policies in South
    Korea](#malaria-key-policies-in-south-korea)

# Introduction

This project analyzes malaria infection trends in South Korea from
**2001 to 2023**. Our research question was: “How have malaria patterns
in South Korea changed over this period?” To answer this, we compared
malaria trends in neighboring countries and examined infection rates in
key regions, particularly **Seoul, Gyeonggi, Gangwon, and Incheon**.

Using animated maps and line graphs, we visualized the changes in
infection rates and identified regions with consistently high malaria
cases. By comparing these patterns with the implementation of key
policies, we aimed to provide recommendation that could help minimize
future malaria infections.

This study provides

-   Valuable insights for malaria prevention strategies 

-   Offers practical implications for future health policy development
    in South Korea

# Global Malaria Overview

Malaria remains a major global health issue, creating continuous
challenges for public health systems worldwide. One important factor
contributing to this issue is **climate change**, which has increased
mosquito activity. The World Health Organization (WHO) reported that 249
million malaria cases occurred globally in 2022, marking a 6.9% increase
compared to 2019, before the COVID-19 pandemic. The WHO warned that this
rise has made eradicating malaria more difficult.

# 1. Analysis of Malaria Incidence Cases in Neighboring Countries

At first, we tried to find countries that were as close to Korea as
possible. However, we found that malaria did not occur in Japan or
Russia after research. In addition, China has not had any cases of
malaria for four consecutive years since 2017. Therefore, we selected
Cambodia, Laos, the Philippines, Thailand, and Vietnam, which are not
only near South Korea but also have reliable data on malaria cases. The
data was sourced from the [Malaria Atlas
Project](https://data.malariaatlas.org/trends?year=2022&metricGroup=Malaria&geographicLevel=admin0&metricSubcategory=Pf&metricType=rate&metricName=incidence).

The original data provided the number of cases per 1,000 people. To
calculate the **actual number of malaria infections**, we multiplied the
incidence rate by the population of each country and divided the result
by 1,000.

<div class="plotly html-widget html-fill-item" id="htmlwidget-1a7dba9b7953c49a10cf" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-1a7dba9b7953c49a10cf">{"x":{"data":[{"x":[2018,2019,2020,2021,2022],"y":[19234,17062,4027,1342,1517],"text":["Year: 2018<br />Malaria.Cases: 19234<br />Name: Cambodia","Year: 2019<br />Malaria.Cases: 17062<br />Name: Cambodia","Year: 2020<br />Malaria.Cases:  4027<br />Name: Cambodia","Year: 2021<br />Malaria.Cases:  1342<br />Name: Cambodia","Year: 2022<br />Malaria.Cases:  1517<br />Name: Cambodia"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(160,32,240,0.8)","dash":"dot"},"hoveron":"points","name":"Cambodia","legendgroup":"Cambodia","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2018,2019,2020,2021,2022],"y":[12889,5732,4068,3296,1195],"text":["Year: 2018<br />Malaria.Cases: 12889<br />Name: Laos","Year: 2019<br />Malaria.Cases:  5732<br />Name: Laos","Year: 2020<br />Malaria.Cases:  4068<br />Name: Laos","Year: 2021<br />Malaria.Cases:  3296<br />Name: Laos","Year: 2022<br />Malaria.Cases:  1195<br />Name: Laos"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,0,255,0.8)","dash":"dot"},"hoveron":"points","name":"Laos","legendgroup":"Laos","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2018,2019,2020,2021,2022],"y":[13840,15017,14362,10186,8029],"text":["Year: 2018<br />Malaria.Cases: 13840<br />Name: Philippines","Year: 2019<br />Malaria.Cases: 15017<br />Name: Philippines","Year: 2020<br />Malaria.Cases: 14362<br />Name: Philippines","Year: 2021<br />Malaria.Cases: 10186<br />Name: Philippines","Year: 2022<br />Malaria.Cases:  8029<br />Name: Philippines"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,100,0,0.8)","dash":"dot"},"hoveron":"points","name":"Philippines","legendgroup":"Philippines","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2018,2019,2020,2021,2022],"y":[1212,4753,362,223,308],"text":["Year: 2018<br />Malaria.Cases:  1212<br />Name: Thailand","Year: 2019<br />Malaria.Cases:  4753<br />Name: Thailand","Year: 2020<br />Malaria.Cases:   362<br />Name: Thailand","Year: 2021<br />Malaria.Cases:   223<br />Name: Thailand","Year: 2022<br />Malaria.Cases:   308<br />Name: Thailand"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(255,0,0,0.8)","dash":"dot"},"hoveron":"points","name":"Thailand","legendgroup":"Thailand","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2018,2019,2020,2021,2022],"y":[5453,5656,2214,446,700],"text":["Year: 2018<br />Malaria.Cases:  5453<br />Name: Vietnam","Year: 2019<br />Malaria.Cases:  5656<br />Name: Vietnam","Year: 2020<br />Malaria.Cases:  2214<br />Name: Vietnam","Year: 2021<br />Malaria.Cases:   446<br />Name: Vietnam","Year: 2022<br />Malaria.Cases:   700<br />Name: Vietnam"],"type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,139,139,0.8)","dash":"dot"},"hoveron":"points","name":"Vietnam","legendgroup":"Vietnam","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2018,2019,2020,2021,2022],"y":[576,559,385,294,420],"text":["Year: 2018<br />Malaria.Cases: 576<br />Name: South Korea","Year: 2019<br />Malaria.Cases: 559<br />Name: South Korea","Year: 2020<br />Malaria.Cases: 385<br />Name: South Korea","Year: 2021<br />Malaria.Cases: 294<br />Name: South Korea","Year: 2022<br />Malaria.Cases: 420<br />Name: South Korea"],"type":"scatter","mode":"lines","line":{"width":4.5354330708661417,"color":"rgba(0,0,0,0.9)","dash":"solid"},"hoveron":"points","name":"South Korea","legendgroup":"South Korea","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":50.271481942714821,"r":9.2984640929846432,"b":53.739999778436825,"l":69.73848069738483},"font":{"color":"rgba(0,0,0,1)","family":"","size":18.596928185969279},"title":{"text":"<b> Incidence Cases of Countries Around Korea <\/b>","font":{"color":"rgba(0,0,0,1)","family":"","size":21.253632212536321},"x":0.5,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[2017.8,2022.2],"tickmode":"array","ticktext":["2018","2019","2020","2021","2022"],"tickvals":[2018,2019,2020,2021,2022],"categoryorder":"array","categoryarray":["2018","2019","2020","2021","2022"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":4.6492320464923198,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":14.877542548775427},"tickangle":-45,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"Year","font":{"color":"rgba(0,0,0,1)","family":"","size":18.596928185969279}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-727.55000000000007,20184.549999999999],"tickmode":"array","ticktext":["0","5000","10000","15000","20000"],"tickvals":[0,4999.9999999999991,10000,15000.000000000002,20000],"categoryorder":"array","categoryarray":["0","5000","10000","15000","20000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":4.6492320464923216,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":14.87754254877543},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"Cases","font":{"color":"rgba(0,0,0,1)","family":"","size":18.596928185969279}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":13.283520132835205},"title":{"text":"Country","font":{"color":"rgba(0,0,0,1)","family":"","size":15.940224159402241}}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"27e9447ac917":{"x":{},"y":{},"colour":{},"type":"scatter"},"27e959282820":{"x":{},"y":{},"colour":{}}},"cur_data":"27e9447ac917","visdat":{"27e9447ac917":["function (y) ","x"],"27e959282820":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

By hovering over each point, you can see the *Year* and *Malaria Cases*,
along with *Country Name*.

Neighboring countries generally showed a declining trend in malaria
cases, while South Korea remained stable with very low rates. However,
**after 2021**, there was a slight increase in South Korea’s incidence.

Specifically, *Cambodia and Laos* exhibited the most notable decreases.
Their incidence rates sharply declining from 2018, reaching near zero by
2022. *The Philippines* displayed a gradual decline, while *Thailand*
had a slight increase before stabilizing around 2020. *Vietnam*
maintained consistently low levels, and **South Korea** consistently
reported minimal cases. Overall, most countries showed declining trends,
particularly Cambodia and Laos, while South Korea’s strong public health
system kept its rates low and stable.

## Why we select the South Korea?

We focused on South Korea because of the slight increase in malaria
cases after 2021, which highlights the need for closer analysis and
improved prevention strategies. Furthermore, the Korea Centers for
Disease Control and Prevention recently announced that the malaria
infections are on the rise and are expected to reach 650 cases this
year.

For our South Korea-specific analysis, we used data from the [Infectious
Disease Portal](https://dportal.kdca.go.kr/pot/is/rgin.do) spanning from
2001 to 2023. After analyzing the data, we found that dividing the data
into distinct time periods revealed significant variations in malaria
incidence. Interestingly, these changes aligned closely with the
implementation of malaria control policies.

# 2. Number of malaria infections by ‘Seoul’, ‘Gyeonggi’, ‘Gangwon’, ‘Incheon’(2001-2023)

<div class="plotly html-widget html-fill-item" id="htmlwidget-bb8d6e8c9919189cb50a" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-bb8d6e8c9919189cb50a">{"x":{"data":[{"orientation":"v","width":[0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095,0.90000000000009095],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[2556,1799,1171,864,1369,2051,2227,1052,1345,1772,826,542,445,638,699,673,515,576,559,385,294,420,747],"text":["Year: 2001 <br>Infections: 2556","Year: 2002 <br>Infections: 1799","Year: 2003 <br>Infections: 1171","Year: 2004 <br>Infections: 864 <br>Policy: Military and civilian control strengthened","Year: 2005 <br>Infections: 1369","Year: 2006 <br>Infections: 2051","Year: 2007 <br>Infections: 2227","Year: 2008 <br>Infections: 1052","Year: 2009 <br>Infections: 1345","Year: 2010 <br>Infections: 1772","Year: 2011 <br>Infections: 826 <br>Policy: Mosquito surveillance enhanced","Year: 2012 <br>Infections: 542","Year: 2013 <br>Infections: 445","Year: 2014 <br>Infections: 638","Year: 2015 <br>Infections: 699","Year: 2016 <br>Infections: 673","Year: 2017 <br>Infections: 515","Year: 2018 <br>Infections: 576","Year: 2019 <br>Infections: 559 <br>Policy: The First Basic Plan (Rapid diagnostic kits introduced)","Year: 2020 <br>Infections: 385","Year: 2021 <br>Infections: 294","Year: 2022 <br>Infections: 420","Year: 2023 <br>Infections: 747"],"type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(204,204,204,0.6)","line":{"width":1.8897637795275593,"color":"transparent"}},"name":"전국","legendgroup":"(전국,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[545,216,132,65,78,123,125,109,154,184,93,12,27,16,20,27,12,11,15,12,8,15,29],"text":"","type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"강원","legendgroup":"(강원,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[909,756,518,399,660,869,1007,490,611,818,382,257,228,311,417,381,295,325,294,227,175,236,434],"text":"","type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(124,174,0,1)","dash":"solid"},"hoveron":"points","name":"경기","legendgroup":"(경기,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[370,285,170,136,213,272,314,126,178,290,93,66,45,96,91,97,68,82,100,57,40,60,94],"text":"","type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(0,191,196,1)","dash":"solid"},"hoveron":"points","name":"서울","legendgroup":"(서울,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[275,267,166,107,222,465,484,164,164,256,122,140,84,131,108,84,80,82,87,48,46,63,126],"text":"","type":"scatter","mode":"lines","line":{"width":3.7795275590551185,"color":"rgba(199,124,255,1)","dash":"solid"},"hoveron":"points","name":"인천","legendgroup":"(인천,1)","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[545,216,132,65,78,123,125,109,154,184,93,12,27,16,20,27,12,11,15,12,8,15,29],"text":["Year: 2001 <br>Infections: 545","Year: 2002 <br>Infections: 216","Year: 2003 <br>Infections: 132","Year: 2004 <br>Infections: 65 <br>Policy: Military and civilian control strengthened","Year: 2005 <br>Infections: 78","Year: 2006 <br>Infections: 123","Year: 2007 <br>Infections: 125","Year: 2008 <br>Infections: 109","Year: 2009 <br>Infections: 154","Year: 2010 <br>Infections: 184","Year: 2011 <br>Infections: 93 <br>Policy: Mosquito surveillance enhanced","Year: 2012 <br>Infections: 12","Year: 2013 <br>Infections: 27","Year: 2014 <br>Infections: 16","Year: 2015 <br>Infections: 20","Year: 2016 <br>Infections: 27","Year: 2017 <br>Infections: 12","Year: 2018 <br>Infections: 11","Year: 2019 <br>Infections: 15 <br>Policy: The First Basic Plan (Rapid diagnostic kits introduced)","Year: 2020 <br>Infections: 12","Year: 2021 <br>Infections: 8","Year: 2022 <br>Infections: 15","Year: 2023 <br>Infections: 29"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(248,118,109,1)","opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(248,118,109,1)"}},"hoveron":"points","name":"NA","legendgroup":"(강원,1)","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[909,756,518,399,660,869,1007,490,611,818,382,257,228,311,417,381,295,325,294,227,175,236,434],"text":["Year: 2001 <br>Infections: 909","Year: 2002 <br>Infections: 756","Year: 2003 <br>Infections: 518","Year: 2004 <br>Infections: 399 <br>Policy: Military and civilian control strengthened","Year: 2005 <br>Infections: 660","Year: 2006 <br>Infections: 869","Year: 2007 <br>Infections: 1007","Year: 2008 <br>Infections: 490","Year: 2009 <br>Infections: 611","Year: 2010 <br>Infections: 818","Year: 2011 <br>Infections: 382 <br>Policy: Mosquito surveillance enhanced","Year: 2012 <br>Infections: 257","Year: 2013 <br>Infections: 228","Year: 2014 <br>Infections: 311","Year: 2015 <br>Infections: 417","Year: 2016 <br>Infections: 381","Year: 2017 <br>Infections: 295","Year: 2018 <br>Infections: 325","Year: 2019 <br>Infections: 294 <br>Policy: The First Basic Plan (Rapid diagnostic kits introduced)","Year: 2020 <br>Infections: 227","Year: 2021 <br>Infections: 175","Year: 2022 <br>Infections: 236","Year: 2023 <br>Infections: 434"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(124,174,0,1)","opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(124,174,0,1)"}},"hoveron":"points","name":"NA","legendgroup":"(경기,1)","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[370,285,170,136,213,272,314,126,178,290,93,66,45,96,91,97,68,82,100,57,40,60,94],"text":["Year: 2001 <br>Infections: 370","Year: 2002 <br>Infections: 285","Year: 2003 <br>Infections: 170","Year: 2004 <br>Infections: 136 <br>Policy: Military and civilian control strengthened","Year: 2005 <br>Infections: 213","Year: 2006 <br>Infections: 272","Year: 2007 <br>Infections: 314","Year: 2008 <br>Infections: 126","Year: 2009 <br>Infections: 178","Year: 2010 <br>Infections: 290","Year: 2011 <br>Infections: 93 <br>Policy: Mosquito surveillance enhanced","Year: 2012 <br>Infections: 66","Year: 2013 <br>Infections: 45","Year: 2014 <br>Infections: 96","Year: 2015 <br>Infections: 91","Year: 2016 <br>Infections: 97","Year: 2017 <br>Infections: 68","Year: 2018 <br>Infections: 82","Year: 2019 <br>Infections: 100 <br>Policy: The First Basic Plan (Rapid diagnostic kits introduced)","Year: 2020 <br>Infections: 57","Year: 2021 <br>Infections: 40","Year: 2022 <br>Infections: 60","Year: 2023 <br>Infections: 94"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(0,191,196,1)","opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(0,191,196,1)"}},"hoveron":"points","name":"NA","legendgroup":"(서울,1)","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023],"y":[275,267,166,107,222,465,484,164,164,256,122,140,84,131,108,84,80,82,87,48,46,63,126],"text":["Year: 2001 <br>Infections: 275","Year: 2002 <br>Infections: 267","Year: 2003 <br>Infections: 166","Year: 2004 <br>Infections: 107 <br>Policy: Military and civilian control strengthened","Year: 2005 <br>Infections: 222","Year: 2006 <br>Infections: 465","Year: 2007 <br>Infections: 484","Year: 2008 <br>Infections: 164","Year: 2009 <br>Infections: 164","Year: 2010 <br>Infections: 256","Year: 2011 <br>Infections: 122 <br>Policy: Mosquito surveillance enhanced","Year: 2012 <br>Infections: 140","Year: 2013 <br>Infections: 84","Year: 2014 <br>Infections: 131","Year: 2015 <br>Infections: 108","Year: 2016 <br>Infections: 84","Year: 2017 <br>Infections: 80","Year: 2018 <br>Infections: 82","Year: 2019 <br>Infections: 87 <br>Policy: The First Basic Plan (Rapid diagnostic kits introduced)","Year: 2020 <br>Infections: 48","Year: 2021 <br>Infections: 46","Year: 2022 <br>Infections: 63","Year: 2023 <br>Infections: 126"],"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgba(199,124,255,1)","opacity":1,"size":7.559055118110237,"symbol":"circle","line":{"width":1.8897637795275593,"color":"rgba(199,124,255,1)"}},"hoveron":"points","name":"NA","legendgroup":"(인천,1)","showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":44.825238688252384,"r":7.3059360730593621,"b":43.105022831050235,"l":53.466168534661698},"font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":14.611872146118724},"title":{"text":"<b> Malaria Infections in South Korea (2001-2023) <\/b>","font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":18.596928185969279},"x":0.5,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1999.405,2024.595],"tickmode":"array","ticktext":["2000","2005","2010","2015","2020"],"tickvals":[2000,2005,2010,2015,2020],"categoryorder":"array","categoryarray":["2000","2005","2010","2015","2020"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.6529680365296811,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"nanumgothic","size":13.283520132835205},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"Year","font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":15.940224159402241}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-127.80000000000001,2683.8000000000002],"tickmode":"array","ticktext":["0","1000","2000"],"tickvals":[1.4210854715202004e-14,1000,2000.0000000000002],"categoryorder":"array","categoryarray":["0","1000","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.6529680365296811,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"nanumgothic","size":13.283520132835205},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"Number of Infections","font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":15.940224159402241}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":13.283520132835205},"title":{"text":"Country","font":{"color":"rgba(0,0,0,1)","family":"nanumgothic","size":15.940224159402241}}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"27e940677383":{"x":{},"y":{},"fill":{},"text":{},"type":"bar"},"27e947d0be7d":{"x":{},"y":{},"colour":{}},"27e9598a2560":{"x":{},"y":{},"colour":{},"text":{}}},"cur_data":"27e940677383","visdat":{"27e940677383":["function (y) ","x"],"27e947d0be7d":["function (y) ","x"],"27e9598a2560":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.20000000000000001,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>

The line graph above illustrates malaria infections in South Korea from
2001 to 2023, highlighting trends at the national level and in the four
most affected regions: **Seoul, Gyeonggi, Gangwon, Incheon**. By
hovering over each point, you can see the *Year* and *Number of
Infections*, along with specific *policies* implemented in key years
(2004,2011,2019).

## Overall Trends

-   Malaria infections across South Korea showed a significant decline
    from 2001 to 2004, motivated by early control measures. This was
    followed by fluctuations during the mid-2000s (2005-2009),
    indicating challenges in sustaining initial successes. From 2010 to
    2017, infections stabilized at lower levels due to enhanced
    surveillance and targeted interventions. However, after 2018, with
    slight increases, signaling the need for continued vigilance.

## Regional Trends

-   <span class="green-text">Gyeonggi</span> consistently reported the
    highest number of infections, contributing significantly to national
    totals.

-   <span class="purple-text">Incheon</span> and
    <span class="blue-text">Seoul</span> exhibited relatively low
    infection rates but followed similar patterns of decline and
    stabilization.

-   <span class="red-text">Gangwon</span> maintained consistently low
    infection rates throughout the period.

# Malaria Key Policies in South Korea

Malaria in South Korea has changed significantly since its peak in the
early 1990s, especially in areas near the DMZ. Over the years, the
government’s focused policies have played an important role in lowering
infection rates, and the implementation of major policies has clearly
impacted trends, as indicated in the line graph above.

**2000s: Early Military & Civilian Control** (2004 Policy)

-   Responding to the rise of malaria, early efforts focused on
    **monitoring and controlling outbreaks** among military personnel
    and local populations near high-risk zones.

-   This policy led to a sharp decline in infections nationwide,
    particularly between 2001 and 2004.

**2010s: Strengthening Surveillance** (2011 Policy)

-   The government enhanced **mosquito surveillance** and introduced
    **epidemiological investigations** to track cluster infections.

-   These efforts improved prevention measures, particularly in
    high-burden regions such as Gyeonggi, where a steady decline is
    observed since 2011.

-   Additionally, **real-time monitoring** and immediate responses to
    localized outbreaks helped limit further transmission.

**2019-2023: The First Basic Plan** (2019 Policy)

-   In this period, **rapid diagnostic kits** introduced. It covered by
    insurance, allowing faster detection and treatment of malaria cases.

-   **Clinical guidelines** were published to standardize diagnosis and
    care.

**2024-2028: Malaria Elimination Action Plan**

The upcoming policy focuses on eliminating malaria by 2028. It include
key strategies:

-   Implementing an **active surveillance system** to detect
    asymptomatic cases.

-   Expanding treatment with medications like **Primaquine** to
    eliminate infections.

As mentioned earlier in the Overall Trends section, we have divided the
malaria infection trends in South Korea into four distinct periods:
2001-2004, 2005-2009, 2010-2017, and 2018-2023. This segmentation allows
for a clearer and more systematic analysis of changes over time and the
impact of key policies.

To make these trends even easier to visualize, we created animated maps
where the color changes based on infection cases during each period.
This visual tool will help you quickly identify how malaria infections
have evolved over the years.

Now, let’s dive deeper into Part 2 for further insights!
