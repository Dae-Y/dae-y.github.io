---
layout: single
title: "A Lucky Detour into Microsoft Fabric"
category: [data-engineering]
tag: Microsoft
author_profile: false
use_math: true
sidebar:
    nav: "counts"
---


During the Semester 1 break, I travelled to Melbourne to see my sister and take a short break after finishing my Honours presentation. While I was there, I also happened to attend a networking event and joined a KDA meetup in Melbourne.

That is where I first heard about **Microsoft Data Days 2026**.

Through the event, I found out that there was a chance to study Microsoft Fabric for free and potentially receive a free exam voucher. I had already joined Microsoft AI Skills Fest 2026 and received a voucher for the **Azure AI Fundamentals (AI-900)** exam, which I am planning to sit soon. Now, quite unexpectedly, I have also started studying for **DP-700** because the Fabric Data Engineer path looked genuinely interesting.

Recently, I have been looking for internships and graduate roles to build professional experience after finishing Honours in 2026. I have always been interested in AI and machine learning, but data science and data engineering have also started to catch my attention more.

Microsoft Fabric feels surprisingly relevant to that direction.

It combines data engineering, analytics, notebooks, Spark, SQL, and other data workloads in one environment. For someone with my background in machine learning, data mining, and database systems, a lot of the concepts already feel familiar enough that I can connect them to things I have studied before.

I started working through a Microsoft Learn module and found that Fabric provides a 60-day free trial. It felt like a good opportunity to actually use the platform instead of only reading about it. My plan is to make good use of the trial period, work through the learning materials, and hopefully prepare properly for the DP-700 exam.

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2026-07-07-MS_Learn/2026-07-07-fabric01.png" style="width: 90%;" />
</div>

The notebook experience was also very relatable. It reminded me a lot of working in Jupyter Notebook. I could write code, run cells, inspect the output, and work with data interactively. Spark DataFrames also felt familiar because of my previous machine learning and data mining experience, while the SQL side connected back to what I learned in my Database Systems unit.

One of the main ideas I learned was that **Microsoft Fabric provides a unified SaaS platform for working with data and analytics**.

Instead of having separate systems constantly moving and copying data between them, Fabric uses **OneLake** as a central storage layer. Different analytics workloads can access the same underlying data, which makes collaboration between data engineers, data scientists, and analysts much easier.

My simple way of understanding it is:

> OneLake is like a shared data foundation for the whole platform.

The data can remain in one logical lake, while different tools and teams work with it for different purposes. I found this idea quite powerful because it reduces the need to constantly move or duplicate data across separate systems.

I also practised working with data in a **Spark DataFrame**.

The workflow was quite straightforward: load data such as a CSV file, transform it using operations like `select`, `filter`, and `groupBy`, and then save the result in a format such as Parquet. Partitioning can also be used when appropriate to improve how large datasets are stored and processed.

Spark DataFrames feel similar to Pandas DataFrames at a high level, but the important difference is scale.

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2026-07-07-MS_Learn/2026-07-07-fabric02.png" style="width: 90%;" />
</div>

Pandas usually processes data in the memory of a single machine. Spark DataFrames are designed to distribute work across multiple worker nodes, which makes them much more suitable for large-scale data processing.

That connection was probably the most interesting part for me. Concepts I had previously seen separately in machine learning, data mining, databases, and HPC now started to overlap in one platform.

The more I use Fabric, the more I understand why data engineering is such an important area. It is not only about storing data. It is about building the infrastructure and workflows that allow data to be processed, analysed, and used reliably by different people and systems.

I still have a lot to learn, but so far, Fabric feels like a very powerful platform and I am genuinely enjoying the learning process.

It is also funny to think that this started from a short trip to Melbourne, attending one networking meetup, and hearing about a free learning opportunity.

Sometimes one small event leads to another.

For now, I want to make the most of the Fabric trial, keep studying, and see how far I can get with DP-700.
