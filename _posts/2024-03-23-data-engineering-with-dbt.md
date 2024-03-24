---
layout: posts
title:  "Data Engineering with DBT"
date:   2024-03-23
tags: [Data Engineering]
highlight_home: true
author_profile: true
author: Kenneth Infante
categories: article
tagline: "Using DBT Core Applying the Medallion Architecture to Parking Violations Data"
header:
  overlay_image: https://images.unsplash.com/photo-1528802704307-fb7d0f29ad0d?q=80&w=2938&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  teaser: https://images.unsplash.com/photo-1528802704307-fb7d0f29ad0d?q=80&w=2938&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  caption: "Photo credit: [**Arisa Chattasa**](https://unsplash.com/@golfarisa)"
description: This article shows how to use DBT Core applying the medallion architecture to parking violations data.
---

## Background

Previously, I watched and blogged about the [**End-to-End Data Engineering Project**](https://www.linkedin.com/learning/end-to-end-data-engineering-project) course at Linkedin Learning. The course is very practical and shows how to build a data pipeline using modern tools and techniques.

Now, there's another Linkedin Learning tutorial that dives deeper about **dbt** - the [**Data Engineering with DBT**](https://www.linkedin.com/learning/data-engineering-with-dbt) course.

## Project Setup

The course project centers around the creation of data models using the [NYC DOF Parking Violation Codes](https://data.cityofnewyork.us/Transportation/DOF-Parking-Violation-Codes/ncbg-6agr/about_data) dataset. This dataset is huge so the course uses only a sample of the dataset.

The course uses the Python and SQL inside a Github workspace to show the use of *dbt* in building the data models. In addition, it also showed how to use Github Actions to automate deployment of the data architecture.


## Results

What I like about this course is that it tackles the following aspects in building a data architecture

* **Medallion architecture** - a model of data architecture. This model is very intuitive as it divides the data models into bronze, silver, and gold. The **bronze** models are basically the raw data, **silver** models are the transformed or cleaned data, and the **gold** models are basically the metrics which are useful for ad-hoc reports and cross-checking of reported amounts.

![Medallion architecture]({{ site.url }}{{ site.baseurl }}/assets/images/data-engineering-with-dbt/overview_medallion.png)

* **Testing** - the course also showed how to test or validate your data when building a data architecture. In my experience, this is critical as it may point out to operational issues in the business. For example, a test can be made to point out any invoice that changed amounts after a week. An invoice that changed amount after a week may signify returns that were made outside the allowed return period, items that were shipped after an invoice was paid, or worse, collusion and theft among employees.

* **Documentation** - the course also showed how to create a maintainable documentation for the data models. In my experience, lack of documentation can result to duplication of SQL queries as a new analyst tries to understand existing architecture, employees keeping copies of outdated information as they may find accessing the data cumbersome and confusing, and reports containing different values for the same KPI or measure as they may use different filters or criteria for the latter.

* **Deployment** - the course also showed how to use Github Actions in order to automate deployments of data models. This is also critical in actual practice. Lack of automated deployments can results to wrong information as outdated data is used, and worse, making decisions based on this information.


## Next Steps

I really enjoyed the course. The course solidified my understanding of DBT and I'm confident to use it on my next projects. I'm looking for ways to apply my learnings in building data architectures on the cloud using *dbt*.