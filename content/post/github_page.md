---
title: "How to Build a Personal Webpage"
date: 2020-08-27T16:21:35+08:00
draft: false
---

Hello, this post is an introduction on how to build a personal website using [Github Pages](https://pages.github.com/) and [Hugo](https://gohugo.io/).

Hugo is one of the most popular framework for building websites. Github Pages is a static web hosting service provided by Github. Here I listed all the steps of building this website.

##  Step 1 - Install Hugo  

The Hugo website has detailed tutorial for installing Hugo. On Mac machines, you can simply install it using the following command:

```{shell}
brew install hugo
```
You can check your installation using the command 'hugo version'. If the version pops up on your command window you should be good to go.

## Step 2 - Setup
Set up a working directory on your local machine, mine is called '/projects', and within the '/projects' folder create another folder to save your website changes. Mine is called '/blog'.  Inside the '/blog' folder, initiate the Hugo server via the following command:

```{shell}
$ hugo new site .
```

Log in to your Github page and create a repository to connect to the '/blog' folder on your machine. It saves all changes you make locally with your website. Here is the command to connect to the Github repository:

```{shell}
$ git init
$ git remote add origin # the https url of the repository you just created #
$ git pull origin master
```
Then push your changes to the repository and all the files should be there.  


## Step 3 - Choose a theme

The [themes](https://themes.gohugo.io/) section of Hugo contains many beautiful themes of the website for you to choose from. I chose [Ananke](https://themes.gohugo.io/gohugo-theme-ananke/) and clicked the Homepage button to get to its Github page.  

The next thing is to add this theme to your website. Simply git clone the Github page of the theme into the '/themes' folder under the '/blog' folder on your local machine.

## Step 4 - Build your website locally

The most important features of the webpage is inside the `config.toml` file under '/blog'. You can use the text editor you like to edit the file. (I used *Atom*.) Use the following command under the '/blog' to create blogs/posts:

```{shell}
$ hugo new post/# file name #.md
```
The .md file should be in the '~/blog/content/post' folder. You can now write the blog in this file. Once finished, go to '/blog' folder again and type the following command:

```{shell}
$ hugo server -w
```
For my project the following information pops up:

```{shell}
| EN  
-------------------+-----
Pages            | 11  
Paginator pages  |  0  
Non-page files   |  0  
Static files     |  6  
Processed images |  0  
Aliases          |  1  
Sitemaps         |  1  
Cleaned          |  0  

Built in 18 ms
Watching for changes in /Users/chenxiwu/Desktop/2020 Fall/BIOS 823/projects/blog/{archetypes,content,data,layouts,static,themes}
Watching for config changes in /Users/chenxiwu/Desktop/2020 Fall/BIOS 823/projects/blog/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
You can check your website by clicking the localhost link above. After making sure everything looks good, push your changes to the blog repository using the following commands:

```{shell}
$ git add .
$ git commit -m "made some changes"
$ git push origin master
```

## Step 5 - Deploy the site

Create a Github repository named as **username** + 'github.io' and git clone it to the '/projects' folder. Then do "git pull origin master" to keep your branch up to date. My repository name is 'cassie1102.github.io'. Under '/blog' folder, type:

```{shell}
$ hugo -d ../cassie1102.github.
$ cd ../cassie1102.github.io
```
Then use git add, commit and push the changes to the repository. Under the 'Settings' button of the repository, you can see the link to your personal website within the 'Github Pages' section.

This is all the processes I went through to build this website. Please contact me via cassiew1102@gmail.com for further questions.
