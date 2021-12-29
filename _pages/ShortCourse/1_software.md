---
layout: page
title: Software
sub-title: The DFT Short Course
sidebar_link: True
sidebar_sort_order: 1
permalink: /ShortCourse/software.html
---

The programs engaged in this document were chosen as favorites (or in some cases, relics) of Collum group members.  There is a large emphasis on those that are open source and free to use (**listed in bold below**), however there are a plethora of alternatives available across the internet and the following list should be taken as a starting point, not a superlative.

### You'll need the following programs for this course

[Cornell VPN client](https://it.cornell.edu/articles/topics/2605/all/822) (for Cornell students who intend to use the AS-CHEM cluster)  
SFTP client (e.g. **FileZilla**, **putty**)  
Molecular modeling program (e.g. GaussView, **Avogadro**, SAMSON, etc...)  
Command-line terminal (e.g. the built in **terminal** on Linux/Mac OS or **cygwin** if you're using a Windows machine)  
Command-line text editor (e.g. **Vim**, **Vi**, **Nano**; helpful, but not required)  
A text editor (e.g. **Brackets**, Sublime, **VSCode**, **Notepad++**, etc...)  
3D-rendering/ray-tracing software, optional (e.g. **CYLView**, SAMSON, etc)  

<br>

### FileZilla

[<kbd>FileZilla</kbd>](https://filezilla-project.org/) is an SFTP (**s**ecure **f**ile **t**ransfer **p**rotocol) client that we'll use to move files between our own computers and the cluster; we'll have to do this since we can't just wander over to the Baker server room and stick a USB drive into the stacks every time we want our data.

#### Setup

>If you haven't talked to your system admin to set yourself up a cluster account then stop here and get that done first. You'll need a fully set up account (on our cluster or your own) to get the most out of this course.  

These setup instructions are for the AS-CHEM cluster only, if you're not trying to connect to this cluster, contact your system administrator for instructions on how to transfer files to/from your own system.  

0. [Install and connect to the Cornell VPN](https://it.cornell.edu/articles/topics/2605/all/822)
1. Download the [latest <kbd>FileZilla</kbd> release](https://filezilla-project.org/download.php?type=client). Follow [these instructions](https://wiki.filezilla-project.org/Client_Installation) to install it.  
2. Start FileZilla and open the site manager by going to File > Site Manager.  
3. Add a new connection by clicking "New Site"
    <center>
        <img src="/dftCourse/assets/1_1.png" width="634.2" height="280.2">
    </center>  
4. Configure the settings for the new site as shown:
    <center>
        <img src="/dftCourse/assets/1_2.png" width="634.2" height="280.2">
    </center>
    - Protocol: `SFTP - SSH File Transfer Protocol`
    - Host: `cluster2020.chem.cornell.edu`
    - Logon type: `Normal` or `Ask for password` if you want to be asked every time
    - User: Your cluster logon (typically your netID)
    - Password: If you selected “Normal” for logon type then enter your cluster password.  If “Ask for password” then you will be prompted for your password every time you connect.
5. Click `Connect` to connect to the cluster and make sure you have everything set up correctly. Your cluster home directory should show up on the right.

<center>
    <img src="/dftCourse/assets/1_3.png" width="641.5" height="462.5">
</center>

To reconnect in the future just go back to the site manager, select the cluster site and click `Connect`. Remember that you need to be connected to the Cornell VPN to access the cluster.

### Command-line terminals

If you are working on a Mac or Linux machine feel free to skip this part. Linux distributions and MacOS come preinstalled with a command-line terminal.  

Technically, Windows machines come with <kbd>PowerShell</kbd> (which is its own shell and scripting language (two things you'll learn about in the next lesson)), but for the sake of uniformity and since the cluster runs on CentOS, we'll use a <kbd>bash</kbd> shell. Windows users should download and install [<kbd>cygwin</kbd>](https://www.cygwin.com/). **By default cygwin does not come with text editors installed** (don't ask me why) so you'll have to go through the setup.exe program to install these packages (start with [<kbd>vim</kbd>](https://cygwin.com/packages/summary/vim.html) and [<kbd>nano</kbd>](https://cygwin.com/packages/summary/nano.html)). This [blog post](https://wilsonericn.wordpress.com/2011/08/15/cygwin-setup-gotchas/) may come in handy.  

### Molecular modeling programs

#### GaussView

Due to licensing constraints you'll need to visit [ChemIT](https://it.chem.cornell.edu/) to get <kbd>GaussView</kbd> installed on your computer.

#### Avogardo

[<kbd>Avogadro</kbd>](https://avogadro.cc/) is a free, open-source molecular editor and visualizer that can be used as an alternative to <kbd>GaussView</kbd>. It comes [fully documented](https://avogadro.cc/docs/) and has built-in compatibility with <kbd>Gaussian</kbd> input adn output. Download and install it from [SourceForge](https://sourceforge.net/projects/avogadro/files/latest/download).

### Text Editors

#### Brackets

[<kbd>Brackets</kbd>](https://brackets.io/) is an open-source, lightweight text editor that was built for web designers and front-end developers, but it has access to a the local filesystem a really helpful feature in keeping our files organized. Check out the [<kbd>Brackets</kbd> Wiki](https://github.com/brackets-cont/brackets/wiki) to learn more (always start with the README).

#### VSCode

[<kbd>Visual Studio Code</kbd>](https://code.visualstudio.com/) is what's known as an IDE, for **i**ntegrated **d**evelopment **e**nvironment.  It is open-source, but not lightweight. <kbd>VS Code</kbd> is super powerful, with built-in features like line-by-line debugging, intelligent code completion, built-in terminal, expanded language support, and spell check. If you're just getting started, *this is not the text editor you're looking for*, but as you become more experienced you may find that you want more than a simple code editor. [Read the docs](https://code.visualstudio.com/docs) to get started.  

### 3D-rendering and ray-tracing

#### CYLView

[<kbd>CYLView</kbd>](http://cylview.org/) is a free molecular visualizer currently in development by Claud Y. Legault at the University of Sherbrook (Canada). Invaluable for drawing publication-quality chemical structures from computational output. Both versions <kbd>CYLView1.0</kbd> and <kbd>CYLView20</kbd> can be downloaded [here](http://cylview.org/download.html). Mac users will need to install [<kbd>XQuartz</kbd>](https://www.xquartz.org/) in order to use <kbd>CYLView1.0</kbd>. Its recommended that you install both versions as <kbd>CYLView20</kbd> is still pretty barebones.  

<br />

| <center><a href="/dftCourse/introduction.html">Home</a></center> | <center>Next<br><a href="/dftCourse/ShortCourse/linuxBasics.html">Linux Basics</a></center> |

<br />
