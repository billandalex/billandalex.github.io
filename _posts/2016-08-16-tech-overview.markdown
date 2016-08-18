---
layout: post
title:  "Technical Organization"
date:   2016-08-16 08:05:14 -0700
categories: banda
summary: A summary of the technical organization of this project
---

![Technical Organization Chart]({{ site.baseurl }}/images/tech-overview-chart.jpg)

## [Banda Theme](https://github.com/billandalex/banda-theme)

This is the main project repository, although it is not Wordpress theme. That still needs to be developed. Rather, it contains a wp-content folder (along with plugins, themes, and uploads subfolder) and some other configuration that can be dropped into a Wordpress installation and will result in a ready-to-go development environment for what will eventually be the final theme package. This development environment can be automatically set up using banda-installer.

## [Banda Installer](https://github.com/billandalex/banda-installer)

This is a script to quickly install a local development site for the Banda Theme. It combines the latest WordPress version with the Banda theme and its media files. The script then deletes itself and all its components.

## [Banda UI](https://github.com/billandalex/banda-ui)

This is a style/pattern guide for Banda Theme. It contains discreet and repeatable UI patterns, built on top of Bootstrap. Built with [Fabricator](http://fbrctr.github.io/).

## [Banda Styles](https://github.com/billandalex/banda-styles)

This repo provides the SCSS for both Banda Theme and Banda UI, which both include this repo as a submodule. In this way, SCSS changes to UI components can be made in one place and shared between the pattern guide and theme. When a change to this repo is pushed to master, the Banda Theme and Banda UI submodules can be pointed to the latest commit to see the updates.

## [billandalex.github.io](https://github.com/billandalex/billandalex.github.io)

The repo for this website. A jekyll site published via Github Pages.
