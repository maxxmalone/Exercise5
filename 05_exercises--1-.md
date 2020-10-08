---
title: 'Weekly Exercises #5'
author: "Phebe Chen"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(ggimage)
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  
1st graph: Exercise 2 number 2.

```r
garden_plotly_graph <- garden_harvest %>% 
  filter(vegetable%in%c("lettuce")) %>%
  mutate(variety = fct_rev(fct_infreq(variety))) %>% 
  ggplot()+
  geom_bar(aes(y=variety,
               text=variety),
           fill="pink")+
labs(title="Number of times each variety of lettuce was harvested",
       y="",
       x="")

ggplotly(garden_plotly_graph,
         tooltip = c("count", "text"))
```

<!--html_preserve--><div id="htmlwidget-d506b1ab5a20c61f72af" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-d506b1ab5a20c61f72af">{"x":{"data":[{"orientation":"v","width":[1,3,9,27,28],"base":[0.55,1.55,2.55,3.55,4.55],"x":[0.5,1.5,4.5,13.5,14],"y":[0.9,0.9,0.9,0.9,0.9],"text":["count:  1<br />mustard greens","count:  3<br />reseed","count:  9<br />Tatsoi","count: 27<br />Farmer's Market Blend","count: 28<br />Lettuce Mixture"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,192,203,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":133.698630136986},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Number of times each variety of lettuce was harvested","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-1.4,29.4],"tickmode":"array","ticktext":["0","10","20"],"tickvals":[0,10,20],"categoryorder":"array","categoryarray":["0","10","20"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,5.6],"tickmode":"array","ticktext":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"tickvals":[1,2,3,4,5],"categoryorder":"array","categoryarray":["mustard greens","reseed","Tatsoi","Farmer's Market Blend","Lettuce Mixture"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"902b5939ee02":{"y":{},"text":{},"type":"bar"}},"cur_data":"902b5939ee02","visdat":{"902b5939ee02":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2nd graph: Exercise 3 number 4.


```r
garden_plotly_graph2 <- garden_harvest %>% 
  group_by(vegetable, variety, date) %>% 
  mutate(wt_lb=weight*0.00220462) %>%
  summarize(tot_harvest=sum(wt_lb)) %>%
  filter(vegetable %in% c("tomatoes")) %>% 
  arrange(date) %>% 
  ggplot(aes(y=variety, x=tot_harvest))+
  geom_col(fill="pink")+
  labs(title="Total harvest in pounds for each variety",
       y="Variety",
       x="Total Harvest (lbs)")

ggplotly(garden_plotly_graph2)
```

<!--html_preserve--><div id="htmlwidget-cd531d9c6c374b29c4df" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-cd531d9c6c374b29c4df">{"x":{"data":[{"orientation":"v","width":[0.05291088,0.30203294,0.74736618,0.18959732,0.4850164,0.3086468,0.54454114,0.06834322,1.02073906,0.23368972,0.32628376,1.76590062,0.68784144,0.44753786,0.28880522,0.6944553,1.34702282,0.46076558,0.97444204,0.33730686,0.5291088,0.0881848,0.20062042,0.43431014,0.6393398,0.67681834,0.220462,1.39552446,0.21384814,0.96121432,0.9590097,0.7054784,1.36465978,0.37037616,1.12215158,0.46517482,1.88935934,0.34392072,0.22487124,0.74075232,0.67902296,0.85318794,0.50926722,0.26014516,0.74736618,0.16093726,0.84216484,0.49163026,1.24120106,0.6393398,0.47840254,1.72180822,0.14770954,0.3858085,0.67681834,0.86641566,0.66799986,2.3038279,0.79145858,0.78484472,0.64374904,1.23899644,0.80248168,0.51367646,0.35714844,1.24340568,0.17857422,0.40565008,2.42949124,0.39462698,0.67902296,0.14109568,1.30293042,0.11904948,0.47619792,0.53131342,1.11553772,0.78043548,0.79145858,0.67681834,0.48060716,1.76810524,0.66579524,0.3527392,1.41536604,1.60275874,0.91050806,0.73193384,0.928145020000001,1.1574255,0.58642892,1.56748482,0.277782119999999,0.39903622,0.52469956,1.0802638,1.2345872,0.52469956,0.87523414,0.64154442,1.19710866,1.05160374,1.32056738,0.72311536,1.29631656,1.68432968,0.80248168,0.6724091,0.96121432,0.67461372,0.69886454,0.59745202,0.29982832,1.34040896,0.2314851,0.32628376,2.19800614,1.3558413,0.58201968,1.27647498,0.7385477,1.92242864,0.994283619999999,0.67461372,2.2266662,0.507062600000001,0.75838928,1.39331984,1.06483146,1.08687766,0.7936632,0.73413846,0.943577360000001,1.85629004,3.39070556,0.727524600000001,3.52959662,0.584224300000001,0.535722659999999,1.23899644,3.39952404,3.46786726,0.593042779999999,0.98326052,0.96121432,1.55205248,1.76590062,0.1653465,3.086468,2.18918766,1.38670598,1.92022402,1.11553772,1.27427036,2.26194012,0.253531300000001,1.07585456,4.15791332,0.68122758,1.09569614,0.476197920000001,2.29721404,1.30733966,1.80558378,1.46827692,0.575405819999999,2.27737246,0.837755599999999,2.41846814,2.87261986,6.46835508,0.866415659999999,0.341716100000003,1.0141252,1.32056738,1.83644846,1.65787424,1.29852118,1.89817782,1.81219764,3.38850094,0.392422359999999,0.44312862,1.70417126,2.64995324,1.7747191,4.30562286,2.7888443,2.49342522,1.3448182,4.7619792,6.39119338,0.9479866,0.97444204,2.59704236,0.562178099999997,2.72050108,2.90348454,3.63541838,1.5652802,1.3558413,5.24038174,1.52559703999999,3.06883104,1.12215158,1.66228348,0.696659920000002,1.48591388,0.910508060000002,0.568791959999999,1.5983495,0.467379439999998,0.502653360000004,1.4770954,2.31926024,1.57409868,3.59573522,0.670204479999999,2.33248796,6.46835508,1.57409868,0.209438900000002,6.03624956,2.73152418,1.45284458,0.520290319999999,1.05160374,1.80558378,4.42246772,4.01902226,4.36073836],"base":[7.55,2.55,4.55,7.55,1.55,4.55,6.55,7.55,0.55,7.55,4.55,9.55,1.55,2.55,7.55,8.55,10.55,0.55,1.55,4.55,6.55,7.55,7.55,0.55,1.55,6.55,7.55,10.55,0.55,3.55,4.55,5.55,6.55,7.55,0.55,1.55,3.55,4.55,7.55,10.55,1.55,4.55,5.55,7.55,9.55,11.55,0.55,2.55,4.55,5.55,7.55,9.55,11.55,0.55,2.55,3.55,6.55,1.55,4.55,5.55,7.55,8.55,9.55,10.55,2.55,4.55,7.55,8.55,1.55,4.55,6.55,7.55,8.55,11.55,6.55,8.55,0.55,1.55,3.55,4.55,5.55,6.55,7.55,11.55,0.55,1.55,2.55,4.55,7.55,0.55,2.55,4.55,7.55,8.55,10.55,11.55,0.55,1.55,2.55,3.55,4.55,7.55,10.55,11.55,0.55,1.55,4.55,5.55,7.55,11.55,3.55,4.55,7.55,9.55,10.55,11.55,0.55,1.55,2.55,3.55,5.55,6.55,7.55,11.55,0.55,1.55,2.55,4.55,5.55,7.55,8.55,11.55,0.55,2.55,3.55,4.55,6.55,7.55,10.55,11.55,0.55,3.55,4.55,5.55,7.55,9.55,10.55,7.55,0.55,2.55,4.55,5.55,7.55,8.55,9.55,10.55,11.55,0.55,1.55,2.55,3.55,5.55,6.55,7.55,10.55,11.55,2.55,7.55,9.55,11.55,0.55,1.55,4.55,5.55,6.55,7.55,8.55,9.55,10.55,11.55,0.55,5.55,6.55,8.55,9.55,10.55,11.55,2.55,7.55,11.55,0.55,1.55,5.55,7.55,8.55,9.55,11.55,0.55,2.55,4.55,7.55,11.55,0.55,1.55,7.55,8.55,9.55,10.55,11.55,7.55,11.55,11.55,0.55,1.55,4.55,5.55,10.55,2.55,7.55,11.55,4.55,11.55,0.55,1.55,2.55,3.55,4.55,7.55,9.55,10.55,11.55],"x":[0.02645544,0.15101647,0.37368309,0.14770954,0.2425082,0.90168958,0.27227057,0.27667981,0.51036953,0.42769628,1.21915486,0.88295031,0.82893712,0.52580187,0.68894375,0.34722765,0.67351141,1.25112185,1.66007886,1.55095017,0.80909554,0.87743876,1.02184137,1.69865971,2.46696978,1.41205911,1.23238258,2.04478505,2.02273885,0.48060716,2.19910845,0.3527392,2.43279817,1.52780166,2.69073871,3.01922709,1.90589399,2.85057366,1.82542536,3.11292344,3.59132598,3.44912799,0.96011201,2.06793356,2.13958371,0.08046863,3.67289692,0.99538593,4.49632249,1.53441552,2.43720741,3.37417091,0.23479203,4.28688359,1.57961023,3.28378149,3.44912799,5.08275141,5.51265231,2.24650778,2.9982832,1.31395352,4.63631586,3.74013783,2.09659362,6.53008444,3.40944483,2.13627678,7.44941098,7.34910077,4.1226394,3.56927978,2.99056703,0.36817154,4.70024984,3.90768895,5.0375567,9.05437434,4.11271861,7.88482343,2.87923372,5.82240142,3.97272524,0.60406588,6.30300858,10.24597145,2.73042187,8.58919952,4.76969537,7.58940435,3.47889036,9.73890885,5.37265894,4.37286377,4.25932584,1.32056738,8.7854107,11.3097006,4.20972189,4.82922011,11.12120559,6.03735187,5.18195931,2.22225696,10.05086258,12.41421522,12.12100076,3.45574185,7.0437609,2.9211215,5.49942459,12.82096761,7.67428222,5.70776118,5.95798555,3.42157024,11.79802393,13.93430071,4.9383488,6.48709435,4.16122025,7.66766836,8.32133819,3.92201898,14.0103601,14.86575266,5.60855328,13.81635354,5.06290983,9.36191883,4.96921348,4.62639507,15.59548188,6.91589294,8.82068462,14.87677576,10.39368099,10.19746981,6.34158943,5.61296252,17.76703258,12.24997103,15.53705945,6.08695582,10.97018912,7.1539919,7.49240107,11.53346953,21.0100286,8.93863179,16.52693383,7.53869809,12.17391164,6.00318026,9.0609882,8.50211703,6.77038802,24.63221926,15.45989775,10.58107369,14.22200362,9.64741712,12.81214913,13.63447239,9.36302114,7.59601821,12.26760799,14.95614208,11.40119233,9.32003105,29.94535346,16.23371937,17.39114487,11.30308674,14.12610265,16.29324411,7.46925256,13.25968699,11.04624851,11.6624398,34.87378147,12.00636052,15.00795065,9.15027531,15.2339242,12.88269697,14.72135005,14.80071637,18.45818095,17.54657058,38.94902154,19.86252389,12.676565,20.19211458,11.30088212,16.83998987,19.57923022,42.78175341,18.01284771,18.34464302,21.35725625,23.55967163,44.9962942,24.5925361,22.59625269,13.43054504,17.46940888,14.51301346,26.63511653,23.44172446,27.88954531,28.92240978,46.0104194,26.86549932,20.28691324,13.93760764,17.05383801,20.16565914,24.89236442,32.39027704,22.2335927,35.72917403,49.27987086,28.96980911,21.22718367,14.72024774,23.54644391,26.96140029,20.0289727,20.86121675,38.01426266],"y":[0.899999999999999,0.9,0.9,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.9,0.899999999999999,0.9,0.899999999999999,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.9,0.9,0.9,0.9,0.899999999999999,0.9,0.899999999999999,0.899999999999999,0.9,0.899999999999999,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999],"text":["tot_harvest: 0.05291088<br />variety: grape","tot_harvest: 0.30203294<br />variety: Big Beef","tot_harvest: 0.74736618<br />variety: Bonny Best","tot_harvest: 0.18959732<br />variety: grape","tot_harvest: 0.48501640<br />variety: Better Boy","tot_harvest: 0.30864680<br />variety: Bonny Best","tot_harvest: 0.54454114<br />variety: Cherokee Purple","tot_harvest: 0.06834322<br />variety: grape","tot_harvest: 1.02073906<br />variety: Amish Paste","tot_harvest: 0.23368972<br />variety: grape","tot_harvest: 0.32628376<br />variety: Bonny Best","tot_harvest: 1.76590062<br />variety: Mortgage Lifter","tot_harvest: 0.68784144<br />variety: Better Boy","tot_harvest: 0.44753786<br />variety: Big Beef","tot_harvest: 0.28880522<br />variety: grape","tot_harvest: 0.69445530<br />variety: Jet Star","tot_harvest: 1.34702282<br />variety: Old German","tot_harvest: 0.46076558<br />variety: Amish Paste","tot_harvest: 0.97444204<br />variety: Better Boy","tot_harvest: 0.33730686<br />variety: Bonny Best","tot_harvest: 0.52910880<br />variety: Cherokee Purple","tot_harvest: 0.08818480<br />variety: grape","tot_harvest: 0.20062042<br />variety: grape","tot_harvest: 0.43431014<br />variety: Amish Paste","tot_harvest: 0.63933980<br />variety: Better Boy","tot_harvest: 0.67681834<br />variety: Cherokee Purple","tot_harvest: 0.22046200<br />variety: grape","tot_harvest: 1.39552446<br />variety: Old German","tot_harvest: 0.21384814<br />variety: Amish Paste","tot_harvest: 0.96121432<br />variety: Black Krim","tot_harvest: 0.95900970<br />variety: Bonny Best","tot_harvest: 0.70547840<br />variety: Brandywine","tot_harvest: 1.36465978<br />variety: Cherokee Purple","tot_harvest: 0.37037616<br />variety: grape","tot_harvest: 1.12215158<br />variety: Amish Paste","tot_harvest: 0.46517482<br />variety: Better Boy","tot_harvest: 1.88935934<br />variety: Black Krim","tot_harvest: 0.34392072<br />variety: Bonny Best","tot_harvest: 0.22487124<br />variety: grape","tot_harvest: 0.74075232<br />variety: Old German","tot_harvest: 0.67902296<br />variety: Better Boy","tot_harvest: 0.85318794<br />variety: Bonny Best","tot_harvest: 0.50926722<br />variety: Brandywine","tot_harvest: 0.26014516<br />variety: grape","tot_harvest: 0.74736618<br />variety: Mortgage Lifter","tot_harvest: 0.16093726<br />variety: volunteers","tot_harvest: 0.84216484<br />variety: Amish Paste","tot_harvest: 0.49163026<br />variety: Big Beef","tot_harvest: 1.24120106<br />variety: Bonny Best","tot_harvest: 0.63933980<br />variety: Brandywine","tot_harvest: 0.47840254<br />variety: grape","tot_harvest: 1.72180822<br />variety: Mortgage Lifter","tot_harvest: 0.14770954<br />variety: volunteers","tot_harvest: 0.38580850<br />variety: Amish Paste","tot_harvest: 0.67681834<br />variety: Big Beef","tot_harvest: 0.86641566<br />variety: Black Krim","tot_harvest: 0.66799986<br />variety: Cherokee Purple","tot_harvest: 2.30382790<br />variety: Better Boy","tot_harvest: 0.79145858<br />variety: Bonny Best","tot_harvest: 0.78484472<br />variety: Brandywine","tot_harvest: 0.64374904<br />variety: grape","tot_harvest: 1.23899644<br />variety: Jet Star","tot_harvest: 0.80248168<br />variety: Mortgage Lifter","tot_harvest: 0.51367646<br />variety: Old German","tot_harvest: 0.35714844<br />variety: Big Beef","tot_harvest: 1.24340568<br />variety: Bonny Best","tot_harvest: 0.17857422<br />variety: grape","tot_harvest: 0.40565008<br />variety: Jet Star","tot_harvest: 2.42949124<br />variety: Better Boy","tot_harvest: 0.39462698<br />variety: Bonny Best","tot_harvest: 0.67902296<br />variety: Cherokee Purple","tot_harvest: 0.14109568<br />variety: grape","tot_harvest: 1.30293042<br />variety: Jet Star","tot_harvest: 0.11904948<br />variety: volunteers","tot_harvest: 0.47619792<br />variety: Cherokee Purple","tot_harvest: 0.53131342<br />variety: Jet Star","tot_harvest: 1.11553772<br />variety: Amish Paste","tot_harvest: 0.78043548<br />variety: Better Boy","tot_harvest: 0.79145858<br />variety: Black Krim","tot_harvest: 0.67681834<br />variety: Bonny Best","tot_harvest: 0.48060716<br />variety: Brandywine","tot_harvest: 1.76810524<br />variety: Cherokee Purple","tot_harvest: 0.66579524<br />variety: grape","tot_harvest: 0.35273920<br />variety: volunteers","tot_harvest: 1.41536604<br />variety: Amish Paste","tot_harvest: 1.60275874<br />variety: Better Boy","tot_harvest: 0.91050806<br />variety: Big Beef","tot_harvest: 0.73193384<br />variety: Bonny Best","tot_harvest: 0.92814502<br />variety: grape","tot_harvest: 1.15742550<br />variety: Amish Paste","tot_harvest: 0.58642892<br />variety: Big Beef","tot_harvest: 1.56748482<br />variety: Bonny Best","tot_harvest: 0.27778212<br />variety: grape","tot_harvest: 0.39903622<br />variety: Jet Star","tot_harvest: 0.52469956<br />variety: Old German","tot_harvest: 1.08026380<br />variety: volunteers","tot_harvest: 1.23458720<br />variety: Amish Paste","tot_harvest: 0.52469956<br />variety: Better Boy","tot_harvest: 0.87523414<br />variety: Big Beef","tot_harvest: 0.64154442<br />variety: Black Krim","tot_harvest: 1.19710866<br />variety: Bonny Best","tot_harvest: 1.05160374<br />variety: grape","tot_harvest: 1.32056738<br />variety: Old German","tot_harvest: 0.72311536<br />variety: volunteers","tot_harvest: 1.29631656<br />variety: Amish Paste","tot_harvest: 1.68432968<br />variety: Better Boy","tot_harvest: 0.80248168<br />variety: Bonny Best","tot_harvest: 0.67240910<br />variety: Brandywine","tot_harvest: 0.96121432<br />variety: grape","tot_harvest: 0.67461372<br />variety: volunteers","tot_harvest: 0.69886454<br />variety: Black Krim","tot_harvest: 0.59745202<br />variety: Bonny Best","tot_harvest: 0.29982832<br />variety: grape","tot_harvest: 1.34040896<br />variety: Mortgage Lifter","tot_harvest: 0.23148510<br />variety: Old German","tot_harvest: 0.32628376<br />variety: volunteers","tot_harvest: 2.19800614<br />variety: Amish Paste","tot_harvest: 1.35584130<br />variety: Better Boy","tot_harvest: 0.58201968<br />variety: Big Beef","tot_harvest: 1.27647498<br />variety: Black Krim","tot_harvest: 0.73854770<br />variety: Brandywine","tot_harvest: 1.92242864<br />variety: Cherokee Purple","tot_harvest: 0.99428362<br />variety: grape","tot_harvest: 0.67461372<br />variety: volunteers","tot_harvest: 2.22666620<br />variety: Amish Paste","tot_harvest: 0.50706260<br />variety: Better Boy","tot_harvest: 0.75838928<br />variety: Big Beef","tot_harvest: 1.39331984<br />variety: Bonny Best","tot_harvest: 1.06483146<br />variety: Brandywine","tot_harvest: 1.08687766<br />variety: grape","tot_harvest: 0.79366320<br />variety: Jet Star","tot_harvest: 0.73413846<br />variety: volunteers","tot_harvest: 0.94357736<br />variety: Amish Paste","tot_harvest: 1.85629004<br />variety: Big Beef","tot_harvest: 3.39070556<br />variety: Black Krim","tot_harvest: 0.72752460<br />variety: Bonny Best","tot_harvest: 3.52959662<br />variety: Cherokee Purple","tot_harvest: 0.58422430<br />variety: grape","tot_harvest: 0.53572266<br />variety: Old German","tot_harvest: 1.23899644<br />variety: volunteers","tot_harvest: 3.39952404<br />variety: Amish Paste","tot_harvest: 3.46786726<br />variety: Black Krim","tot_harvest: 0.59304278<br />variety: Bonny Best","tot_harvest: 0.98326052<br />variety: Brandywine","tot_harvest: 0.96121432<br />variety: grape","tot_harvest: 1.55205248<br />variety: Mortgage Lifter","tot_harvest: 1.76590062<br />variety: Old German","tot_harvest: 0.16534650<br />variety: grape","tot_harvest: 3.08646800<br />variety: Amish Paste","tot_harvest: 2.18918766<br />variety: Big Beef","tot_harvest: 1.38670598<br />variety: Bonny Best","tot_harvest: 1.92022402<br />variety: Brandywine","tot_harvest: 1.11553772<br />variety: grape","tot_harvest: 1.27427036<br />variety: Jet Star","tot_harvest: 2.26194012<br />variety: Mortgage Lifter","tot_harvest: 0.25353130<br />variety: Old German","tot_harvest: 1.07585456<br />variety: volunteers","tot_harvest: 4.15791332<br />variety: Amish Paste","tot_harvest: 0.68122758<br />variety: Better Boy","tot_harvest: 1.09569614<br />variety: Big Beef","tot_harvest: 0.47619792<br />variety: Black Krim","tot_harvest: 2.29721404<br />variety: Brandywine","tot_harvest: 1.30733966<br />variety: Cherokee Purple","tot_harvest: 1.80558378<br />variety: grape","tot_harvest: 1.46827692<br />variety: Old German","tot_harvest: 0.57540582<br />variety: volunteers","tot_harvest: 2.27737246<br />variety: Big Beef","tot_harvest: 0.83775560<br />variety: grape","tot_harvest: 2.41846814<br />variety: Mortgage Lifter","tot_harvest: 2.87261986<br />variety: volunteers","tot_harvest: 6.46835508<br />variety: Amish Paste","tot_harvest: 0.86641566<br />variety: Better Boy","tot_harvest: 0.34171610<br />variety: Bonny Best","tot_harvest: 1.01412520<br />variety: Brandywine","tot_harvest: 1.32056738<br />variety: Cherokee Purple","tot_harvest: 1.83644846<br />variety: grape","tot_harvest: 1.65787424<br />variety: Jet Star","tot_harvest: 1.29852118<br />variety: Mortgage Lifter","tot_harvest: 1.89817782<br />variety: Old German","tot_harvest: 1.81219764<br />variety: volunteers","tot_harvest: 3.38850094<br />variety: Amish Paste","tot_harvest: 0.39242236<br />variety: Brandywine","tot_harvest: 0.44312862<br />variety: Cherokee Purple","tot_harvest: 1.70417126<br />variety: Jet Star","tot_harvest: 2.64995324<br />variety: Mortgage Lifter","tot_harvest: 1.77471910<br />variety: Old German","tot_harvest: 4.30562286<br />variety: volunteers","tot_harvest: 2.78884430<br />variety: Big Beef","tot_harvest: 2.49342522<br />variety: grape","tot_harvest: 1.34481820<br />variety: volunteers","tot_harvest: 4.76197920<br />variety: Amish Paste","tot_harvest: 6.39119338<br />variety: Better Boy","tot_harvest: 0.94798660<br />variety: Brandywine","tot_harvest: 0.97444204<br />variety: grape","tot_harvest: 2.59704236<br />variety: Jet Star","tot_harvest: 0.56217810<br />variety: Mortgage Lifter","tot_harvest: 2.72050108<br />variety: volunteers","tot_harvest: 2.90348454<br />variety: Amish Paste","tot_harvest: 3.63541838<br />variety: Big Beef","tot_harvest: 1.56528020<br />variety: Bonny Best","tot_harvest: 1.35584130<br />variety: grape","tot_harvest: 5.24038174<br />variety: volunteers","tot_harvest: 1.52559704<br />variety: Amish Paste","tot_harvest: 3.06883104<br />variety: Better Boy","tot_harvest: 1.12215158<br />variety: grape","tot_harvest: 1.66228348<br />variety: Jet Star","tot_harvest: 0.69665992<br />variety: Mortgage Lifter","tot_harvest: 1.48591388<br />variety: Old German","tot_harvest: 0.91050806<br />variety: volunteers","tot_harvest: 0.56879196<br />variety: grape","tot_harvest: 1.59834950<br />variety: volunteers","tot_harvest: 0.46737944<br />variety: volunteers","tot_harvest: 0.50265336<br />variety: Amish Paste","tot_harvest: 1.47709540<br />variety: Better Boy","tot_harvest: 2.31926024<br />variety: Bonny Best","tot_harvest: 1.57409868<br />variety: Brandywine","tot_harvest: 3.59573522<br />variety: Old German","tot_harvest: 0.67020448<br />variety: Big Beef","tot_harvest: 2.33248796<br />variety: grape","tot_harvest: 6.46835508<br />variety: volunteers","tot_harvest: 1.57409868<br />variety: Bonny Best","tot_harvest: 0.20943890<br />variety: volunteers","tot_harvest: 6.03624956<br />variety: Amish Paste","tot_harvest: 2.73152418<br />variety: Better Boy","tot_harvest: 1.45284458<br />variety: Big Beef","tot_harvest: 0.52029032<br />variety: Black Krim","tot_harvest: 1.05160374<br />variety: Bonny Best","tot_harvest: 1.80558378<br />variety: grape","tot_harvest: 4.42246772<br />variety: Mortgage Lifter","tot_harvest: 4.01902226<br />variety: Old German","tot_harvest: 4.36073836<br />variety: volunteers"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(255,192,203,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":113.24200913242},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total harvest in pounds for each variety","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-2.614899782,54.912895422],"tickmode":"array","ticktext":["0","10","20","30","40","50"],"tickvals":[0,10,20,30,40,50],"categoryorder":"array","categoryarray":["0","10","20","30","40","50"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Total Harvest (lbs)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,12.6],"tickmode":"array","ticktext":["Amish Paste","Better Boy","Big Beef","Black Krim","Bonny Best","Brandywine","Cherokee Purple","grape","Jet Star","Mortgage Lifter","Old German","volunteers"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12],"categoryorder":"array","categoryarray":["Amish Paste","Better Boy","Big Beef","Black Krim","Bonny Best","Brandywine","Cherokee Purple","grape","Jet Star","Mortgage Lifter","Old German","volunteers"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Variety","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"902b3eff9892":{"x":{},"y":{},"type":"bar"}},"cur_data":"902b3eff9892","visdat":{"902b3eff9892":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
  
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).
  
  Travel time from PARIS EST to Francfort, Metz, Nancy, Reims, Strassbourg, and Stuttgart per year. 


```r
small_trains_animate<-small_trains %>% 
  filter(departure_station %in% c("PARIS EST")) 
```


```r
small_trains_animate %>% 
  ggplot(aes(x = year, 
             y = total_num_trips,
             color=arrival_station)) +
  geom_jitter() +
  labs(title = "Travel times from PARIS EST",
       subtitle = "arrival_station : {closest_state}",
       color = "arrival_station",
       x = "",
       y = "Travel time")+
  transition_states(arrival_station)
```

![](05_exercises--1-_files/figure-html/unnamed-chunk-4-1.gif)<!-- -->




![](smalltrainsanimate.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 
  

  


![](animate_garden_harvest.gif)<!-- -->

## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  




![](mallorcamap.gif)<!-- -->
 
 I prefer this graph over a static map because I think it's a lot easier to visualize where you went on your bike trip in Mallorca and see the elevation as well.  
 
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  




![](panamarace.gif)<!-- -->
  
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  



![](covidanimation.gif)<!-- -->
  
  This graph isn't a very nice looking graph with all the data it shows with the new cases of COVID by week since there are so many states. It starts of nice, but gets really messy as time goes on. 
  
  7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see.
  




![](covid19per10000animation.gif)<!-- -->

This is a good representation of COVID cases per 10,000 because it colors the state by 10,000 making it easy to see which states have a lot of COVID cases and which states don't have as many in the US.  

## Your first `shiny` app

  8. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
## GitHub link

  9. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
