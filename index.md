---
layout: default
title: Home
---

#HW8.1 Visualizations
## Dataset
https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/building_inventory.csv

## Visualization 1
[Click here to see the visualizations](/viz4.html)

## Write-up for Visualization 1
In the two visualizations I submitted, I am exploring a dataset from the UIUC iSchool dataset that contains information about building inventories.

The first visualization is a line chart that illustrates "the relationship between the number of floors in buildings and the count of such buildings" (HW7). Here, the 'Total Floors' is treated as an ordinal variable since the floors are discrete, ordered categories. The 'Number of Buildings' is quantitatively encoded as it represents count data. A data transformation is applied to filter the buildings with valid floor numbers because i observed that some floor numbers are abnormal (i.e., 200).

The second visualization is a bar chart showing "the total square footage of buildings acquired each year" (HW7). 'Year Acquired' is treated as a quantitative variable, reflecting the continuous, numeric nature of years, while the 'Total Square Footage' is also treated as quantitative, representing the aggregated sum of square footage. The color encoding is conditioned on a user's selection (brushing) in the bar chart: selected areas turn 'steelblue', while non-selected areas are 'grey'. This colors are chosen to provide a clear visual distinction between selected and non-selected areas. A data transformation is applied to filter out the years acquired that are positive numbers.

Interactivity is introduced through a brushing feature in the second visualization that allows users to select a range of years. The selection dynamically updates the visualization 1. This interactivity enables users to explore how the distribution of building floors has changed over selected time periods.

## Python code 
import altair as alt

import pandas as pd

alt.data_transformers.disable_max_rows()

url = "https://raw.githubusercontent.com/UIUC-iSchool-DataViz/is445_data/main/building_inventory.csv"

data = pd.read_csv(url)

filtered_data1 = data[(data['Total Floors'] > 0) & (data['Total Floors'] < 10)]

viz1 = alt.Chart(filtered_data1).mark_line().encode(
    x=alt.X('Total Floors:O', title='Number of Floors'), 
    y=alt.Y('count():Q', title='Number of Buildings') 
)

brush = alt.selection(type='interval', encodings=['x'])

filtered_data2 = data[data['Year Acquired'] > 0]

viz2 = alt.Chart(filtered_data2).mark_bar().encode(
    x=alt.X('Year Acquired:Q', title='Year Acquired'),
    y=alt.Y('sum(Square Footage):Q', title='Total Square Footage'),
    color=alt.condition(brush, alt.value('steelblue'), alt.value('grey'))
).add_selection(brush)

viz3=viz1.transform_filter(brush)

viz4 = alt.hconcat(viz3, viz2)

viz4

---
