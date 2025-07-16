---
title: "tAI - Get Terminal Commands Using Natural Language"
subtitle: "A python package to help you get terminal commands using natural language"
description: "Tired of searching for the right terminal command? tAI is here to help you get the right command using natural language."
date: 2025-07-11
draft: false
toc:
  auto: false

code:
  copy: true
  maxShownLines: 50

math: true
fraction: true
images: ["/images/tAI/tAI Blog Social Preview.png"]
featuredImage: "/images/tAI/tAI Blog Thumbnail.png"
featuredImagePreview: "/images/tAI/tAI Blog Thumbnail.png"
author: "Shoaib"

tags: ["AI","Terminal"]
categories: ["Standalone Project"]

lightgallery: true
---

tAI is a simple command line utility tool built on top of Python. Which can generate any commands based on natural language.

<!--more-->

## What is tAI?

**tAI** is a minimalistic command line utility tool that integrates AI inside terminal. The purpose of this CLI tool is to generate **terminal commands** based on **english (or other) language**. 
\
\
**Example**: User says "list all the files in the current directory" and tAI will generate command `ls -al` and paste it on the terminal. Then user can execute the command by pressing `Enter` key (there is also a self execution mode). 
\
\
**That's it! It doesn't do much or less**. It is a simple tool built on top of python to generate commands on natural language.
\
\
{{< image src="/images/tAI/tAI demo.gif" caption="**tAI in action**" >}}

## Why I Built This?

I have really bad memory and I always forgot what command I need to pass. One day I was working with `gcp sdk` and I had to change the current gcp project to another project. And, I completely forgot the command to do so. Then I've to leave my terminal & go to chatgpt website, then ask it, got a response, paste it in the terminal and press enter. This is a huge bottleneck. Instead of leaving the terminal, I should ask it from my terminal and should get the result. That's what sparked my first interest in building `tAI`.

That's why I built `tAI` which I can use without leaving the terminal and open source it so that others can also use it.
\
\
{{< image src="/images/tAI/using tAI Meme.png" caption="**Using `tAI` instead of asking ChatGPT or Google**" >}}


While I was building I wanted to follow the below design principles:

1. `tAI` have to be as **minimal** as possible.
2. It will only **do what it was intended to do**. Means it'll only generate bash command and paste it. No agentic task, no file editing, nothing else.
3. It should **resemble the AI pop up in the cursor**.
4. It should be very **easy to setup and use**.
5. **Nothing should be shared** from the terminal for security reason.

## How to Use It?
Initially I was targeting only for linux or more precise debian based distribution. But after wasting entire week I was not able to build a `.deb` file for my package. Then I tried to build the entire python package as binary which also didn't go well and that's when I ditched all of those ideas and release it as a python package. **Anyone who have python can use it directly. For those who don't have python they first need to install python. But the python version have to be 3.12 or higher** 


### Simple pip install

The easiest way to use `tAI` is to install via `pip`. 

```
pip install tai-textual
```

Now run `tai` command and start using `tAI` directly from the terminal.

> This is not recommended. `pip` will directly install all the package to the global environment and that may caused dependency conflict.

### Using Virtual Environment

This is also similar to the above but instead of installing globally this will first create a virtual environment and then install the package inside it. Therefore all the packages (`tAI` and it's dependency) will be installed in the virtual environment and will not affect the global environment.
\
\
**1. First go to a specific directory where you want to install `tAI`.**
\
**2. Then create a virtual environment.**
\
{{< codeblock bash >}}
python -m venv venv
{{< /codeblock >}}
\
**3. Activate the venv for Unix based system:**
\
{{< codeblock bash >}}
source venv/bin/activate
{{< /codeblock >}}
\
**Or, for Windows:**
```
.\venv\Scripts\activate.bat
```
\
**4. Then install the package.**
\
{{< codeblock bash >}}
pip install tai-textual
{{< /codeblock >}}
\
That's it! Now you can use `tAI` by running `tai` command in the terminal. I know it's a bit more complicated than the simple pip install but it is much more robust. And if you're using `Ubuntu 22` or above you can't direcly install via `pip`. 
\
\
**There is a neat trick which you can use to create a bash function and call the function directly from the terminal**. That function will automatically handle the virtual environment activation. After you've installed the `tAI` package in the virtual env then just copy paste this bash function inside your `.bashrc` file (or `.zshrc` if you're using `zsh`). You'll find this file in the `home` directory (remember this is a hidden file).
\
\
{{< codeblock bash >}}
tai() {
    # Path to the virtual environment's activate script
    VENV_ACTIVATE="/path_where_you_have_created_the_virtual_env/venv/bin/activate"

    # Check if the activate script exists
    if [ -f "$VENV_ACTIVATE" ]; then
        # Activate the virtual environment
        source "$VENV_ACTIVATE"

        # Run the 'tai' command
        # The actual 'tai' executable should be in the venv's bin directory
        command tai

        # Deactivate the virtual environment upon completion
        deactivate
    else
        echo "Error: tAI virtual environment not found at $VENV_ACTIVATE"
    fi
}
{{< /codeblock >}}


### Installing via pipx

This one is for `Linux` or `Ubuntu` user. It is not recommended to install globally via `pip` in a `debian` based distribution. This can corrupt the entire system. This where `pipx` comes into the picture. It creates an isolated environment for each package and installed them there. Instead of creating a virtual environment manually, installing via `pipx` can be handy. 

**1. Install `pipx`**:
```
sudo apt install pipx
```

**2. Install `tAI` via `pipx`**:
```
pipx install tai-textual
```

That's it! Now you can use `tAI` by running `tai` command in the terminal. You don't need to worry about activating the virtual environment. `pipx` will handle everything.

## What Features tAI have?

{{< image src="/images/tAI/tAI Feature edited.png" caption="**All the features of `tAI`**" >}}

Though the principle of `tAI` was to be as minimal as possible. But I've to balance between minimalism yet configurable. That's why I've added many customization features.

### 21+ LLMs

Currently `tAI` supports 21+ LLMs from different provider such as **Google**, **OpenAI**, **Anthropic** and **Open router** for free models. User have the flexibility to choose any of the model they like. They can change the model from the terminal utilizing terminal user interface (thanks to **textual**). I'll be adding more models in future.

### Free Usage without Setting API keys

You'll have access to Free LLMs from the get go. You'll just install the package and start using it without setting any api keys. Currently `tAI` offers total **7 open source models completely free to use.**

### Default LLM 

Set a default LLM so that everytime `tAI` starts it is set to the default. You can set the default LLM either from the `settings` tab or using command.
\
**Command to set default model:**
{{< codeblock bash >}}
tai --default-model openai/gpt-4o
{{< /codeblock >}}
\
**Or, from the settings tab:**

{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/TWMb8O5VtXTlJozmpQK3m.png" caption="**Default model selecting from setting**" >}}

### Inline & Fullscreen Mode

`tAI` have 2 screen modes. One is **inline** mode similar to cursor another one is **fullscreen** mode. This gives the user complete flexibility to use any of the mode as per his liking. Changing between this to mode is also very simple. Either can use direct command to change between **fullscreen** and **inline** or use the `settings` tab to change the mode. You can find this under the `others` section from the `settings`.

\
\
{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/CxFXZ84qlzgUma9G6yErO.png" caption="**Inline mode of `tAI`**" >}}
\
\
{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/FoJOmIhm0BBaw0ro46c9q.png" caption="**Fullscreen mode of `tAI`**" >}}
\
\
{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/ecXKLHI00X9VhpNoipiHx.png" caption="**Changing between fullscreen & inline**" >}}

> It is advised to use the **fullscreen** mode while using `tAI` inside from `tmux`.

### Auto Paste & Execution Mode

There is 2 mode. One is **Auto paste** and another one is **Auto execute**. User can **toggle between this mode using `ctrl+e` keybind**. In the **Auto paste** mode `tAI` generates a command and paste it to the terminal. Then user can see the command and decide if he wants to run it or not. Meanwhile **Auto execute** model runs the command itself. My advice is to use the **Auto execute** mode with caution. Cause LLM can generate a command which can delete system files. You don't want blindly trust LLMs.

### Custom Prompt Config

To make it more customizable and more personalized I've give the user the choice to **customize the system prompt.** User can customize the **system prompt** from the `settings` tab. You can guide the LLM to your personal need. This is completely upto the user. Also you can see the default **system prompt** from this `prompt config` section.

\
\
{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/DXGusMbv208fLe33MGtc1.png" caption="**Customizing System Prompt from Setting**" >}}


### Seting API keys

You have free access to LLM. But it is still a better choice to bring your own API key (paid or free) for better experience. Free LLM will have rate limitation and slow response time. For the best experience bring your own api key. Currently we have 4 providers:
1. `Google` (Gemini models)
2. `OpenAI` (GPT models)
3. `Anthropic` (Claude models)
4. `Open router` (for all model) 

You can set any of the provider's API key from the settings tab or using the command line.

{{< image src="https://cdn-uploads.huggingface.co/production/uploads/65ca6f0098a46a56261ac3ac/ggxXlEefGquXaVubzvXLD.png" caption="**Providing API key from Setting**" >}}
\
\
or using **command line argument** to setup api key.

| Argument            | Type    | Description                                                                                         | Example Usage                                 |
|---------------------|---------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------|
| `--google`          | string  | Set the Google Gemini API key                                                                       | `tai --google YOUR_GOOGLE_API_KEY`                |
| `--openai`          | string  | Set the OpenAI API key                                                                              | `tai --openai YOUR_OPENAI_API_KEY`                |
| `--anthropic`       | string  | Set the Anthropic API key                                                                           | `tai --anthropic YOUR_ANTHROPIC_API_KEY`          |
| `--openrouter`      | string  | Set the OpenRouter API key                                                                          | `tai --openrouter YOUR_OPENROUTER_API_KEY`        |

> There is another option in the `others` tab of `settings`. Where you can set the all the model to use `Open router` api key. Then you only just provide your own `Open router` api key and use every model. But then the default `open router` api key (which was being used for free models) will be disabled.



## Future & Contribution

This is an open source project so I can't make any promises but I'll try my best to add more features or improvement to `tAI`. At this moment I'm working on the following features:

1. **Add more models** (will be adding new `kimi k2` model)
2. **A docker image** (pull the image and start using without any hassle)
3. **Add context** (currently there is no context is added to LLM call, if it makes mistake it won't know, will be adding context so that it knows about previous query and response)

But, I welcome any contribution to this open source project. If you encounter any bug then create an issue, or ask for a new feature or create a PR with new feature. Below I'm adding the project repo.

1. **Github**: [**tAI**](https://github.com/KillerShoaib/tAI)
2. **PyPI**: [**tai-textual**](https://pypi.org/project/tai-textual/)

## Special Thanks

This project is build on top of 2 great library.

1. [**Textual**](https://textual.textualize.io/): This framework was the **MVP**. Everything you see in the terminal is build on top of this library.
2. [**LiteLLM**](https://www.litellm.ai/): This is an API wrapper for all the LLMs. It made my life so much easier.

## Connect With Me

I'm building stuff with AI/LLM. If you want to see my work or want to do a collab with me then you can reach out to me in my socials.

1. **Facebook**: [**Shoaib Hossain**](https://www.facebook.com/shoaib.hossain.298028)
2. **LinkedIn**: [**Shoaib Hossain**](https://www.linkedin.com/in/shoaibhossainkhandakar/)
3. **X**: [**@KillerShoaib__**](https://x.com/KillerShoaib__)

\
And I have also started **writing blogs** about my work related to AI and other stuffs. If you're interested in it, then checkout my blogs.
- **My Personal Blog**: [**AI With Shoaib**](https://killershoaib.github.io/)

\
See my opensource work from Github.

- **Github**: [**KillerShoaib**](https://github.com/KillerShoaib)



