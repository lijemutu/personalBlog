
---
title: "Documenting with a Static Site Generator"
meta_title: "Using a Static Site Generator for a Blog"
description: "A blog documenting the use of a static site generator and markdown files to easily publish on GitHub Pages."
date: 2025-09-17T12:00:00Z
image: ""
categories: ["Projects", "Engineering"]
author: "Erick López"
tags: ["project","jamstack","blog", "hugo"]
draft: false
---

# Introduction
This project is my third iteration using Static Site Generators (SSG). These sites are built with a framework and the content is based on templates with markdown (.md) files, which offer great flexibility for creating written content (titles, subtitles, lists, images, etc.).

In previous projects, I found the simplicity of generating HTML files appealing, as no service or hosting is required.

# Developer Experience

By setting up the local environment (project and scripts) and GitHub Pages (just 2 clicks), the development process stops and content creation begins.

## Local Environment

* Install [Hugo](https://gohugo.io/) and its dependencies (Windows 11 in my case)
* Create a project with `hugo new site <site-name>` and initialize version control with `git init`
* Find an [official theme](https://themes.gohugo.io/) and add it as a submodule
* Customize the site to your liking
  
## GitHub Pages

When you publish your project on GitHub and have your repository public, you can find the Pages menu in the settings section. By selecting the folder where the compiled project is located, GitHub generates a public URL that is accessible.

## Content Creation

Create a new .md file in the "blog" folder, build the site, and commit your changes. The process starts automatically, and within a few minutes, your content is available at the public URL.
