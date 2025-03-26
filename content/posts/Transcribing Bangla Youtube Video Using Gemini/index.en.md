---
title: "Transcribing Bangla Youtube Video With Gemini"
subtitle: "An end to end guide to transcribe bangla youtube video using gemini flash 2"
description: "An end to end guide to transcribe bangla youtube video using gemini flash 2"
date: 2025-03-01T05:43:40+06:00
draft: false
toc:
  auto: false

math: true
fraction: true
images: ["/images/DeepLearning/Chapter1/Deep-Learning-thumbnail-SocialPreview.png"]
featuredImage: "/images/YoutubeTranscriptionGemini/Bangla Youtube Video Transcription Banner.png"
featuredImagePreview: "images/YoutubeTranscriptionGemini/Bangla Youtube Video Transcription Pipeline.png"
author: "Shoaib Hossain"

tags: ["STT","LLM"]
categories: ["Standalone Project"]

lightgallery: true

---

Let's build a **Bangla Youtube Transcriber** using Google's new **Gemini Flash 2.**

<!--more-->


## Why Build This?

I'm working as a professional AI engineer in a startup company. Where We're building customer support system using Gen AI technologies. We're facing problem to find out good Bangla open source LLM.I wouldn't bored you with all the details why I'm doing this but it comes down to mainly one thing, which is **contributing to open source Bangla AI community.** I want to create a domino effect or inspire other so that other can use this project or some thing else to contribute to **Bangla AI community** and eventually we start to have or atleast have some decent resources and dataset for Bangla LLM.

**And, I do think this project is cool or interesting to me. So, yeah this is another reason**

## What We're Building & What to Expect?

I think you can already guessed from the name of it, we're building **Bangla Youtube Transcriber**. It'll basically create a dataset containing the transcribed text from the YouTube audio. Well I'll be honest I can't transcribe whole Bangla videos that are present in YouTube. For this project I've chosen **Rakibul Hasan's** Youtube channel. I've transcribed approximately 60% of his entire youtube channel. "Why 60?"% you may ask. Well the thing is that YouTube is stubborn and it was blocking my automation script that's why I couldn't able to download all the audios. After getting the audios we use **Gemini Flash 2.0** to trancribe each of the audio into text.

In this blog I'll show you my thought process and how I've build this. But this is **not going to be line by line code explanation.** I'll show the overall concept and details so that you can reproduce or build something similar to this. But for the entire code you can check out the [**github repo**](https://github.com/KillerShoaib/BanglaYoutubeTranscribe)


## How to get Gemini API Key?

Since, we're going to use `Gemini Flash 2.0` we need to have `Google API key`. Don't worry google is generous. They are giving **1,500 API request per day for** free without any credit card. All you need to have is google account. Everyone have google account (If you don't then please for the love of god stop living under a rock).

**Step 1.**
Go to [**Google AI Studio**](https://aistudio.google.com/) and login if you've not done it already.

**Step 2.**
Click on the `Get Api Key` button just shown below.

{{< image src="/images/YoutubeTranscriptionGemini/API_ss1.png" caption="**Click on `Get Api Key`**" >}}

**Step 3.**
After that click on the `Create Api key` button.

{{< image src="/images/YoutubeTranscriptionGemini/API_ss2.png" caption="**Click on `Create Api key`**" >}}

**Step 4.**
Finally, copy the API key and save it somewhere else we'll be using it later.

{{< image src="/images/YoutubeTranscriptionGemini/API_ss3.png" caption="**Copy the API key**" >}}


**That's it! We're all set to begin our journey.**

## Illustration of The Complete Workflow

Here, I'll be showing you the abstract level illustration of the complete workflow. Which will give you a better picture how this project is going to be.

First I've divided this entire task into **2 diffrent phases.**

1. **Phase 1:** Downloading the youtube audios
2. **Phase 2:** Generating the transcription


Let's start with **Phase 1**.

### Phase 1 (Youtube Audio Download)

{{< image src="/images/YoutubeTranscriptionGemini/Blog_diagram_1_youtube_pipeline_arc.png" caption="**Phase 1 (Youtube Audio Download)**" >}}

#### Step 1. Save all the playlist urls in a python file as `list`

In this step, I've gone through all the playlists from **Rakibl Hasan's** youtube channel and gathered all the `playlist` url links and manually save them in a python file as `list`. **"Why did I do this manually?"** you may ask. Well, the thing is there weren't lots of playlist in his channel. So doing it manually didn't take too much time.

#### Step 2. Get all video links & save them into a `json` file

Since I've already gathered all the **playlists** url then the next step is to iterate over all the playlists and get all the video links from each playlist. To do this I've used `pytube` a python library which helps to automate many things related to youtube. After getting all the **video urls** then I've saved them into a `json` file where the `key` is the `playlist` name and the `value` is the list of `video urls`.

#### Step 3. Download all the audios

Now we've not only all the playlist url but also all the video urls. **"But, Shoaib WHY DID YOU GATHER ALL OF THESE URLS?"**. Okay okay don't start screaming at me I'm telling you. All of this urls are necessary to download the audios from the Youtube video. Remember `pytube`? This library can directly download audio from any youtube video directly. All we need to do is give that particular youtube video url.

We've gathered all the **video urls**. Now all we need to do is create a python automation script which will download all the audios from the youtube video. This is exactly what I've done in this step. I'll be going to explain the `automation script` part in the coding section.

#### Step 4. Save all the audios in a folder

After downloading all the audios I need to save them in a strcture folder structure so that I can access them later. See the below folder structure

```
└── Audio/
    ├── Playlist_name1/
    │   ├── video_name1.mp3
    │   └── video_name2.mp3
    └── Playlist2/
        ├── video_name1.mp3
        └── video_name2.mp3
```

In the above you can see all the audio had been saved to `Audio` folder. Now inside that audio folder it contains all the `playlist` folders. Each `playlist` folder is named after the **Youtube playlist** name. Inside each `playlist` folder we have `audio` files and each of the `audio` files is named after the **Youtube video** name.

And that's it. That was the end of the **Phase 1**. Now you can take a 5 min break my soldier. Only 5 min, do not make it a 50 min fb scrolling session. 

Okay, break time is over now start the **Phase 2**.

### Phase 2 (Transcription)

{{< image src="/images/YoutubeTranscriptionGemini/Blog_diagram_2_transcription_pipeline_arc.png" caption="**Phase 2 (Transcription)**" >}}

#### Step 1. Playlist & Video Name Json File

This step proved that I'm stupid. Cause this should've been done in the previous phase while automating the `url` links. You see after I completed the previous phase then I realized that I didn't track which video belongs to which playlist. I've **Youtube url** of each video. But don't have the name. That's why in this step I've created another `json` file containing the `playlist` name as key and all the `video names` in a `list` as value. Luckily I had saved all the audio in tree like strucutre in the previous step therefore all I had to do is go through the `Audio` folder and save the `playlist` name and `video name` in the `json` file.

The main purpose of creating this `json` file is that it'll be used to load the audio (I know there is a better way of doing this, stop judging me, I've already told you I'm stupid).



#### Step 2. Load Audio

In this step I load the `json` file which was created in the above step. Then I loop over all the `playlist` and `video` names and load the audio file. But one thing to note here is that **it is an iterative process**. Means I'm not loading all the audio files at once. Loading them all at once will cause memory issues. So, I've loaded the audio file one by one and then do the below steps and after saving trnascription I'll repeat the same process for the next audio file.


#### Step 3. Prompting

This is where I do the prompting. Since `Gemini Flash 2.0` is a **multimodal** model. It can take **text** as well as **audio** as input. I'm passing the audio file along side some instructions in the prompt so that it can generate the transcription. I'll show the detail prompt in the **coding section.** Beside prompting, there was another configuration that I've done. I'm using **Structure Output** from the model. Means the output return by the model will be in a `json` format.


#### Step 4. Calling Gemini Flash 2.0

After prompting we use our api key to call `Gemini Flash 2.0` and generate the transcription.

#### Step 5. Structure Output Response

After calling the api we get the Structure `json` response. Now the question is why use structure output? This can be answered from many different prespective. But, for our usecase suppose when we prompt the model to generate the transcription and it returns the response but also add additional text such as "I'll start transcribing the audio now" or "I'll start processing the audio now". Now this is a pain in the ___ (fill in the blank). We need only the transcription text anything else is going to mess up the transcription process. Yes, in theory we can prompt the model to return only the transcription text. But, there is still high possibility that the model will return some additional text. So, to avoid this we use **structure output**. This ensures that the model will return only the transcription text. And also it becomes easier to handle the response.

#### Step 6. Save The Transcription

Now that I've got the transcription from the above step. All I did is save the transcription along with the `audio` file name and `playlist` name in a `json` file.

#### Step 7. Repeat The Process Untill End

From **Step 2** to **Step 6** I've repeated the process untill I've got the transcription for all the audio files.


## Automating Youtube Audio Download








<!-- 

## Automating Youtube Audio Downloading


### How I had downloaded the youtube audios?

### The Problem I've faced while Automating

### Things to Consider If you want to do it yourselves.


## Creating the Transcription Pipeline

### Why choose Evil Google

### Generating the transcription

### More resources & Future work


## Gratitude -->
 