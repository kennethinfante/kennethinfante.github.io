---
layout: posts
title:  "How to Think of Pandas Data Visualization If You’re Coming From Excel"
date:   2023-11-28
tags: [python, pandas]
highlight_home: false
author_profile: true
author: Kenneth Infante
categories: article
tagline: "Building a Mental Model for Data Visualization in Pandas"
header:
  overlay_image: https://images.unsplash.com/photo-1474631245212-32dc3c8310c6?q=80&w=1924&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  teaser: https://images.unsplash.com/photo-1474631245212-32dc3c8310c6?q=80&w=1924&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  caption: "Photo credit: [**Ricardo Annadale**](https://unsplash.com/@pavement_special)"
description: This article shows how to think of data visualization in Pandas if you're coming from Excel.
---

> This article is originally published in this [link](https://towardsdatascience.com/how-to-think-of-pandas-data-visualization-if-youre-coming-from-excel-7af9f933e212?source=friends_link&sk=d672507887b7e41074ebb33ee742d132) at Medium.com. I updated it here in order to include the [dataset](https://github.com/kennethinfante/pandas_data_visualization_compare_excel) used.


Having read a lot of tutorials on Pandas data visualization, I still can’t grasp the mechanics of it. Creating even a simple plot always requires me to look into the documentation.

And even after running the code and getting the plot right, it doesn’t make me confident to try it on my own. Perhaps, I’m looking for the familiarity of Excel. The connection between the plot and data just seems to be intuitive using the GUI.

With that in mind, can I somehow bring this to Pandas?

## Charting in Excel and Pandas
Consider the following data on the number of immigrants from China and India to Canada from 1980 to 2013 (Note: This data is taken from Kaggle.com. The original [dataset](https://www.kaggle.com/datasets/ammaraahmad/immigration-to-canada/data) contains records from 195 countries.)

![Immigrations from China and India to Canada 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/df_CI_wide_xl.png)

Plotting this data on a line chart in Excel results to

![Excel Line Chart showing Immigrations from China and India to Canada 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/df_CI_wide_line.png)

Alright. Nice and easy.

How about the same data but China and India columns unpivoted to create a single Country column?

![Immigrations to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/df_CI_long_xl.png)

![Excel Line Chart showing Immigrations to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/df_CI_long_line_mising.png)

Ok. That’s a mess.

How about in Pandas? Let's plot the first dataframe

![Pandas Line Chart showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/CI_wide_pd.png)

Ok. That works.

Let's also plot the second dataframe.

![Pandas Line Chart showing Immigrations to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/CI_long_pd.png)

Both the Excel and Pandas showed the same plot for both dataframes.

*It seems Excel and Pandas render plot the same way!* I’m on to something.

## Wide-form vs. Long-form Data

The data we have dealt a while ago are the wide-form and long-form data respectively.

The difference between the two forms are:

* Wide-form data has one row per independent variable, with metadata recorded in the row and column labels.
* Long-form data has one row per observation, with metadata recorded within the table as values.

And this was my aha! moment.

> The wide-form has worked well with line chart because I’m basically plotting an independent variable (year) against its metadata (the India and China series).

Now, will this line of thinking work? Let’s found out.

## Creating Basic Plots with Pandas

Now, let’s try to create different plots for our wide-form data to test my hypothesis.

**Bar Chart**

![Pandas Bar Chart showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/wide_bar_plot.png)

**Area Chart**

![Pandas Area Chart showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/wide_area_plot.png)

**Box Plot** 

![Pandas Box Plot showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/wide_box_plot.png)

**Histogram**

![Pandas Histogram showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/wide_hist_plot.png)

**Scatter Plot**

![Pandas Scatter Plot showing Immigrations from China and India to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/scatter_error.png)

Unfortunately, the scatter plot resulted in an error.

## My Second Aha Moment

So looking back to the previous plots, it’s now making sense.

> If you’re plotting multiple Series against an independent variable, then you use the wide-form. Otherwise, use the long-form.

In other words, the scatter plot is essentially a single chart plotting the number of immigrants on each year. The other charts like bar chart, line chart, etc are essentially multiple charts stacked on one another.

Let's do again the scatter plot using long-form data and differentiate the dots by country.

**Scatter Plot**

![Pandas Scatter Plot showing Immigrations to Canada from 1980 to 2013]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/scatter_plot.png)

Hooray! That works.

## But There are Other Libraries Out There…

I chose to limit the plotting discussed here to the DataFrame's plot method.

When you’re new to Pandas coming From Excel, you want to evaluate quickly if you can reproduce the usual charts that you’re using in Excel to be confident using te new tool.

Besides, effective data analysis hinges with fast creation of plots; plot this, manipulate data, plot again, and so on. Hence, you’re going to be bogged down if you try to incorporate different plotting methods here.

> Take this as Pareto principle as applied to visualization — you only need to know 20% of the plotting techniques to be productive.


## Conclusion
In summary, data in wide-form works well when you’re comparing or plotting multiple Series against the same index. Otherwise, it’s better to stick with the long-form.

The workflow here is that you need to get the data first in the right form and change it until you get the desired plot.

![Feedback loop between the data, plot, and you. ]({{ site.url }}{{ site.baseurl }}/assets/images/pandas-data-viz-from-excel/loop2.png)

Only then you could design or add elements to the plot to make it more appealing.

This is similar to Excel when doing data visualization. You have to get the right data first for Excel to give the right chart. Then you change chart elements, add title, etc. to more it more effective.

Rather than reading a lot of tutorials on Pandas data visualization, having a mental model of how the data corresponds to the plot makes data visualization more fun.
