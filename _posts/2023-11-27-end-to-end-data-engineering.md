---
layout: posts
title:  "End to End Data Engineering Project"
date:   2023-11-27
tags: [Data Engineering]
highlight_home: true
author_profile: true
author: Kenneth Infante
categories: article
tagline: "Modern Data Engineering Stack with Big Query, Airbyte, DBT, & Dagster"
header:
  overlay_image: https://images.unsplash.com/photo-1639392656835-ba117e7b97a1?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  teaser: https://images.unsplash.com/photo-1639392656835-ba117e7b97a1?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  caption: "Photo credit: [**Crystalweed**](https://unsplash.com/@crystalweed)"
description: This article shows how to set up a data pipeline using modern data engineering stack.
---

>

## Background

As I’m looking for ways to learn more about data analytics and engineering, I stumbled upon the [**End-to-End Data Engineering Project**](https://www.linkedin.com/learning/end-to-end-data-engineering-project) course at Linkedin Learning. The course is very practical and shows how to build a data pipeline using modern tools and techniques. The author is using macOS and I’m using Windows. As I followed along the course, I stumbled upon issues with setting up the same pipeline on Windows. Because of this, I decided to write this article to show the steps of setting up the data pipeline on Windows.

The data pipeline is basically made up of the following:

* **Ingestion** - Using Airbyte to load the data from the Postgres database to the data warehouse in Big Query
* **Transformation** - Using DBT to load the data from the raw dataset to the staging dataset in Big Query
* **Orchestration** - Using Dagster in order to link the Ingestion and Transformation steps in a single pipeline

## Project Setup

First, you have to download **Docker Desktop for Windows**. The complete instructions are on this [page](https://docs.docker.com/desktop/wsl/). Make sure that the Docker-WSL integration is enabled. The WSL will enable us to use Linux on Windows. I’ve used the default distribution included in the WSL which is Ubuntu 22.04 on my machine.

![Docker WSL integration]({{ site.url }}{{ site.baseurl }}/assets/images/docker-wsl-integration.png)

Next, we set up our Python environment to use DBT and Dagster for our project. Again, we use the Ubuntu terminal as DBT is throwing some errors when running in Windows.

The tutorial uses `venv` for the creation of a virtual environment. I decided to use `virtualenv` and `virtualwrapper` instead as I want my Ubuntu installation to have the usual Python setup that I use. This way, I can create and switch between different virtual environments inside my Ubuntu installation in case I have another project.

I followed this [tutorial](https://www.freecodecamp.org/news/virtualenv-with-virtualenvwrapper-on-ubuntu-18-04/) in order to set up my Python environment on Ubuntu.

After setting up Ubuntu and Python installation, the other steps are basically the same as the tutorial. However, I’ve used the terminal on our Ubuntu installation to run all the scripts and commands that the author used, instead of the Windows command prompt.

Take note also that the author provided a different docker image for the Postgres database as the original image used in the tutorial is suitable for the Docker app on Apple Silicon architecture. The alternative docker image can be downloaded as follows

```
$ docker run --name big-star-container -d -p 5432:5432 lilearningproject/big-star-postgres-multi:latest -c "wal_level=logical"
```

## Results

Using Ubuntu on Windows allowed me to follow along the tutorial and build a data pipeline using a modern data engineering stack.

## Next Steps

I’m looking to apply my learnings on this tutorial to build another data pipeline but using other tools like Fivetran, AWS, etc. Of course, I’ll write the steps and possibly create a tutorial to showcase alternative architectures.