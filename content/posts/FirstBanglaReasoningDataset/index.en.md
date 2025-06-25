---
title: "How We Created First Bangla Reasoning Dataset"
subtitle: "Bangla reasoning dataset synthetically generated from deepseek-r1"
description: "A complete walkthrough of our process of creating first open source bangla reasoning dataset on top of DeepSeek-r1"
date: 2025-06-22
draft: false
toc:
  auto: true

code:
  copy: true
  maxShownLines: 50

math: true
fraction: true
images: ["/images/DeepSeekBangla/DeepSeek Bangla Reasoning Blog thumbnail.png"]
featuredImage: "/images/DeepSeekBangla/DeepSeek Bangla Reasoning Blog thumbnail.png"
featuredImagePreview: "/images/DeepSeekBangla/DeepSeek Bangla Reasoning Blog thumbnail.png"
author: "Shoaib"

tags: ["LLM","Dataset"]
categories: ["Standalone Project"]

lightgallery: true
---

Time to deep dive to find out how to build a synthetic Bangla reasoning dataset using **DeepSeek-r1**

<!--more-->

## Origin of the Idea

It was in january of this year and suddenly the world was taking over by the **DeepSeek-r1**. It was the first open source reasoning model and
boy oh boy it was good. I was playing with it and instantly fall in love with this model (not that type of love). I was able to solve lots of
the complex problem. In that time only **o1** from openai can come close to its performance. But for me the most interesting thing was the 
reasoning traces. I was able to see all the reasoning that the model was doing before the final answer.


That's when I tried to think what if the model can reason in Bangla? I tried to prompt the model specifically to think in bangla.
But no luck! The model either think in English or Chinese. Then after doing bit of research I got to know that the model was trained in such 
a way that it will think in the dominant language that it was trained on. That's why I failed to prompt it. I'm not going to details of why
is the case, if you want to know more then you can search **GRPO** and how it works.


And here you go that's when I think how 'bout I create a dataset that can be used for **supervised finetuning to mimic the reasoning capabilities**
but the reasoning is happening in the Bangla. And from this idea we started working on creating the first ever Bangla reasoning dataset.

## Dataset & Codebase

For the impatince one who doesn't want to read the blog and just want the dataset or see the code (though I'm warning the codebase is a hell due to my skills issue) I'm providing
them in the begining of the blog.

1. **Bangla Reasoning Dataset** - [**DeepSeek-r1-Distill-Bangla-MMLU-Reasoning-Data**](https://huggingface.co/datasets/KillerShoaib/DeepSeek-r1-Distill-Bangla-MMLU-Reasoning-Data)
2. **Github Repo** - [**DeepSeek-r1-Bangla-Reasoning-Data**](https://github.com/KillerShoaib/DeepSeek-r1-Bangla-Reasoning-Data)


## Where to get the money?

{{< image src="/images/DeepSeekBangla/money-begging.jpg" caption="**Me raising money for the project**" >}}

Okay, the above picture isn't true but still I need to figure out a way how to manage the cost. You see I wanted to use the reasoning traces from **DeepSeek-r1**. Even though their usage from the website is free but I need to use the api which is not free. So my initial budget was 5k BDT. To the
rich people that's no money but man I'm broke. I started working for couple of months (back then) so don't make much to begin with. So I need a partner
where we can split the bill. That's where my man **Numaer** comes into the picture. He was the unsung hero of this project. Not only he split the bill but also setup all the logistics such as billing and etc. And after that he also helped me with the project. He is also a NLP/python guy so our interest and expertise are almost same. In the end we were able to finish the project and the final cost was around 7K BDT. This couldn't possible without the help of **Numaer**. Thanks my man, I owe you.

## How did we do it?



<!-- 
TOC
- So how did we do it?
  - Selecting the base daaset
  - Selecting a subset from it
  - generate the synthetic data
  - Differentiating between correct Vs Wrong
  - Bangla Translation using flash 2
  - Handling Incomplete Translation
- Lots of Limitations
- Ending Words from the creators

 -->

