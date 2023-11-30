---
layout: posts
title:  "PowerQuery Guide to Pandas"
date:   2023-11-28
tags: [PowerQuery, Pandas]
highlight_home: true
author_profile: true
author: Kenneth Infante
categories: work
tagline: "A Comparative Approach to Learn Pandas"
header:
  overlay_image: /assets/images/powerquery-guide-to-pandas/banner.png
  teaser: /assets/images/powerquery-guide-to-pandas/cover.png
  caption: "[**PowerQuery Guide to Pandas**](https://leanpub.com/powerqueryguidetopandas)"
description: This article shows how to leverage your knowledge of PowerQuery to learn Python Pandas
---

## Background

When I first encountered PowerQuery, I’m really amazed on how I can do data cleansing and transformations quickly without having to write a lot of code. I could just use Excel as the GUI to build my solutions and within minutes, I could come up with the desired result for my data. However, PowerQuery is only available back then in Excel 2013 and up, and in PowerBI on the Windows platform. If I need to do any data transformations on another OS, let's say on a Mac, then I have to use another software for this. The best tool I could think of for this is the Pandas library in Python.

To do data transformations in Pandas, you have to write Python code. The downside is, it takes a bit of a learning curve to use it well. In addition, you can’t see your data the same way you see it in PowerQuery after every transformation. The usual practice is to print the dataframe at each step and see if it matches your expected result. It takes time before it becomes "muscle memory". 

To learn Pandas quickly, I realized that I could map the common operations I do in PowerQuery and come up with equivalent Pandas code that I could use. Also, thinking in terms of the PowerQuery interface somehow points me to the right code when using Pandas.

## Approach

To write my book, I use markdown in order to create the draft. This allowed me to focus on writing the book first without being distracted on the formatting. Then I use standard annotations in markdown to format the headings, code snippets, etc in the book. Next, with some Python scripts as well as open sources tools such as Pandoc, I was able to create a PDF, epub, and mobi versions of my draft. With this approach, I was able to continuously refine the book and correct mistakes until it is ready for publishing. Finally, I use Leanpub in order to publish and sell my book. I also distributed and sold dmy book on Gumroad and Amazon

## Results

Since the book was published, I was able to sell 30 copies of the book through Leanpub alone. I also had sales from Gumroad and Amazon. Though not a bestselling book, it is still a good and worthwhile experience. I was able to include this book on my portfolio as a proof of my skills in Python and Excel.

## Next steps

It's been a while since I published the first version of the book. There were a lot of changes to Pandas API with the first and second major release. Python can also be used now directly in Excel (called **Python in Excel**) though still in beta and available only for Microsoft 365 users. There's also the new **Data Wrangler** extension in VS code that mimics PowerQuery functionality. It writes Pandas code with the use of AI out of the point-and-click operations in the GUI.

With this in mind, I'm planning to release a second edition of the book to cover these new trends. 

*Note: The book is still available at [Leanpub](https://leanpub.com/powerqueryguidetopandas).*