# covid
---
title: "COVID 19 IN NIGERIA"
output: 
  flexdashboard::flex_dashboard:
    orientation: rows
    vertical_layout: fill
    social: ["twitter", "facebook", "menu"]
    source_code: embed
---

```{r setup, include=FALSE}
library(flexdashboard)
library(knitr)
library(rpivotTable)
library(ggplot2)
library(rpivotTable)
library(dplyr)
library(openintro)
library(highcharter)
library(DT)
library(plotly)
```

```{r}
covid = read.csv("~/Desktop/covid19.csv")
covid1 = na.omit(covid)
covid2 = covid1
``` 

```{r}
mycolors = c("blue", "green", "darkgreen", "darkorange")
```

Case Summary
==============================================

Row
----------------------------------------------


### State Infection

```{r}
valueBox(sum(covid2$No..of.Cases..Lab.Confirmed.),
         paste("Total Infection"), 
         color = "Grey")
```

### Current Admission

```{r}
valueBox(sum(covid2$No..of.Cases..on.admission.),
         paste("Total on Admission"), 
         color = "Orange")
```

### Total Discharged

```{r}
valueBox(sum(covid2$No..Discharged),
         paste("Total Discharged"),
         color = "Green")
```

### Total Death

```{r}
valueBox(sum(covid2$No.of.Deaths),
         paste("No of Deaths"), 
         color = "Red")
```

Row
-------------------------------------------------------
### COVID-19 TABLE: NIGERIA
```{r}
datatable(covid2,
          caption = "COVID 19 DATA TABLE",
          rownames = T,
          filter = "top",
          options = list(pageLength = 5))
```

Visualization By State
====================================================

```{r}
State_Infection = covid2 %>%
  plot_ly(x = ~States.Affected,
          y = ~No..of.Cases..Lab.Confirmed.,
          color = rainbow(20),
          type = 'bar') %>%
layout(xaxis = list(title = "State with Infection"),
       yaxis = list(title = "Number of Infection in States"),
       showlegend = F)
State_Infection
```

Deaths By State
==================================================
```{r}
State_Death = covid2 %>%
  plot_ly(x = ~States.Affected,
          y = ~No.of.Deaths,
          color = "red",
          type = 'bar') %>%
layout(xaxis = list(title = "State with Covid Death"),
       yaxis = list(title = "Number of States"),
       theme_bw())
State_Death
```

Pivot Table
==================================================
```{r}
rpivotTable(covid2,
            aggregatorName = "No..of.Cases..Lab.Confirmed.",
            rows = "States.Affected",
            cols = "sum(No..of.Cases..Lab.Confirmed.)",
            rendererName = "Heat Map")
```

About Report
==================================================

This App is created by Akinlalu Akintunde.

Data is provided by the NCDC



