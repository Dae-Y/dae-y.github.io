---
layout: single
title: "Quick Guide to SSH into Computer Labs"
category: coursework
tag: 
author_profile: false
sidebar:
    nav: "counts"
---

Steps to remotely access Lab 314.232 machines for use in computing units.

## 0. Preface

This guide outlines the steps to remotely access Linux machines in lab 314.232. 
It will cover connecting via the Curtin VPN, setting up VS Code for SSH access, and configuring a Python environment with Miniconda.

## 1. Curtin Network VPN

If you're on campus using the student Wi-Fi, you can skip this part.

[supportu.curtin.edu.au](https://supportu.curtin.edu.au/sp)

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/01.png" style="width: 80%;" />
</div>

Search for “Cisco VPN” and follow the appropriate installation articles for your operating system (Windows or Mac).

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/02.png" style="width: 80%;" />
</div>

Once installed, connect to the Curtin network.

## 2. Setting Up VS Code

Install the following extensions in VS Code:

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/03.png" style="width: 80%;" />
</div>

Install the following extensions in VS Code:
- Remote Explorer, SSH

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/04.png" style="width: 60%;" />
</div>

Click the “New Remote” button and enter the following command:

```bash
ssh 12345678@lab232-a01.cs.curtin.edu.au
or
ssh yourstudentID@lab232-b03.cs.curtin.edu.au
```

Select the default config location, connect to the remote, and choose "Linux" from the drop-down menu.

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/05.png" style="width: 90%;" />
</div>

Then, enter your Curtin student password.

## 3. Set Up Python Environment

We’ll use **Miniconda** to manage Python and data science libraries.

### Step 1: Download the Installer

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

### Step 2: Run the Installer

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

Follow the prompts. You can install it to a custom path like `~/miniconda3` for modularity.

### Step 3: Initialize Conda

```bash
source ~/miniconda3/bin/activate
conda init bash  # use zsh/fish/etc. if applicable
```

Restart your terminal if needed.

### Create and Activate Environment

```bash
conda create --name comp3007 python=3.10
conda activate comp3007
```

Your terminal prompt will now look like:

```bash
(comp3007) yourname@yourmachine:~$
```

### Install Packages into the Environment

Once your comp3007 environment is active, you can install the necessary Python packages.
For example, to install a core Python library like NumPy:

```bash
conda install numpy
```

Alternatively, if you have a `requirements.txt` file listing all your project's dependencies, you can install them all at once:

```bash
# Fundamental scientific computing library
numpy

# Plotting and visualization
matplotlib

# Image processing library (often used for opening/saving images)
pillow

# Advanced image processing algorithms
scikit-image

# OpenCV for computer vision tasks
opencv-python

# ... and more
```

```bash
pip install -r requirements.txt
```

## 4. Useful Commands

Check installed packages:

```bash
conda list
```

Check available environments:

```bash
conda info --envs
```

Exit the SSH session:

```bash
Ctrl + D
```

## Bonus: View Jupyter Notebooks Online

Use this web viewer to render `.ipynb` files directly in the browser:

[https://ipynb.js.org/](https://ipynb.js.org/)


## 5. Reference

- Sonny's guide: "Set up miniconda3 and remotely access lab machines"
- [Harry's guide](https://youreonmy.framer.website/blog/guide-to-ssh-with-vscode-into-curtin-uni-computer-science-labs)
- Data Mining lab sheet 1

Thanks for reading 😊