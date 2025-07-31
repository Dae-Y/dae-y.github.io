---
layout: single
title: "Quick Guide to SSH into Computer Labs"
category: technology
tag: 
author_profile: false
sidebar:
    nav: "counts"
---

How to remotely access lab machines for computing units that require GPU Linux machines in lab 314.232.

## 0. Preface

This guide outlines the steps for students in computing units who need to remotely access Linux machines in lab 314.232. It covers connecting via the Curtin VPN, setting up VS Code for SSH access, and configuring a Python environment with Miniconda to support tasks in fields like Machine Perception, and Data Mining.

## 1. Curtin Network VPN

If you're on campus using the student Wi-Fi, you can skip this part.

[supportu.curtin.edu.au](https://supportu.curtin.edu.au/sp)

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/01.png" style="width: 70%;" />
</div>

Search for “Cisco VPN” and follow the appropriate installation articles for your operating system (Windows or Mac).

<div style="text-align: left; margin-bottom: 5px;">
  <img src="{{site.url}}/images/2025-07-25-CSLab_guide/02.png" style="width: 70%;" />
</div>

Once installed, connect to the Curtin network.

## 2. Setting Up VS Code

Install the following extensions in VS Code:

- Remote - SSH
- Python

Click the “New Remote” button and enter the following command:

```bash
ssh 12345678@lab232-a01.cs.curtin.edu.au
```

Or for another machine:

```bash
ssh yourstudentID@lab232-b03.cs.curtin.edu.au
```

Select the default config location, connect to the remote, and choose "Linux" from the drop-down menu.

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

To install packages:

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
- [Harry's guide: Guide to SSH with VS Code into Curtin University Computer Science Labs](https://youreonmy.framer.website/blog/guide-to-ssh-with-vscode-into-curtin-uni-computer-science-labs)
- Data Mining lab sheet 1

