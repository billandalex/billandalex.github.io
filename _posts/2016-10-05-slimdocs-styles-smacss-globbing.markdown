---
layout: post
title:  "SMACSS, Fabricator, Globbing and beyond"
date:   2016-10-05 13:35:14 -0700
categories: banda
summary: The methodologies, frameworks, and decisions behind SlimDocs Styles 
--- 

In an effort to keep SlimDocs Styles maintainable, scalable, and organized, we've decided to adopt the Scalable and Modular Architecture for CSS methodology, aka '[SMACSS](https://smacss.com/)'. SMACSS is a popular methodology developed by Jonathan Snook, a former front-end developer at Yahoo! Mail. Alternative CSS writing methodologies include [SUITCSS](http://suitcss.github.io/), [BEM](http://getbem.com/), [OOCSS](http://oocss.org/), and [Atomic](https://github.com/nemophrost/atomic-css),  all of which seem like solid choices for writing and organizing CSS files, but for this project, we'll be using SMACSS. 


### SMACSS Overview

SMACSS divides SASS files into five categories: 

* base
* layout
* modules 
* state
* theme

__Base__ files will set the site's base HTML elements and their pseudo-classes. Examples of elements in a base SASS file are `html`, `body`, `a`, `a:hover`, `h1`, `h2`, `h3`, `blockquote`, etc.

__Layout__ files determine the positioning of elements on a page. All layout classes and ids are prefixed with the letter 'l' and a dash. Examples of layout classes: `.l-header`, `l-navbar`, `l-footer`, etc.

__Modules__ make up the bulk of the SASS files. Modules are any compartmentalized feature of a website. Examples modules include sidebar, modal, card, etc. Each module is broken into a single file called a 'partial'. 

__State__ files include states that describe the state a module is in. State classes and ids are prefixed with 'is-'. For instance, an active form field might have the class `.is-active` or `.is-hidden`.

__Theme__ files are for styles that augment the default theme. Theme files are useful in applications that allow users to choose the 'skin' of the application, similar to the way Yahoo! Mail allows users to set the theme of their mail interface. SMACSS's theme files are beyond the scope of typical WordPress themes and this project.

### Media Queries in SMACSS

SMACSS recommends keeping media queries bundled with the classes and id's they apply to. For instance, if we were writing a sidebar module, the media queries that apply to the sidebar would be just under the base sidebar styles. 

For more on SMACSS, check out [smacss.com](https://smacss.com/).


## The Current SlimDocs Styles Setup

For this project, we've made a stylesheet into a submodule that plugs into both a WordPress theme (SlimDocs Theme -- full-fledged WordPress theme) and a pattern library (SlimDocs UI -- a UI toolkit built with [Fabricator](https://fbrctr.github.io/)). 

The theme's styles are added directly to the submodule stylesheet, which, in turn, is used to style the pattern library. We can then add the necessary HTML to display the CSS in the pattern library. This allows us to keep pattern library and theme styles synced throughout the project. 

Using the SMACSS methodology, we have five main CSS categories with corresponding folders: __base__, __layout__, **module**, **state**, and **vendor**.

We've removed __Theme__ and added a __Vendor__ folder to house all third-party SASS files, which, at the time, consists of Bootstrap SASS files.

## Globbing with SMACSS

Moving from a single SASS file to smaller, modular SASS files typically means adding many lines of imports to the main SASS file. Every new module, layout, state, base, or vendor partial requires its own import line. To avoid this, we've added 'globbing' to our [SASS preprocessor](https://github.com/billandalex/wp-gulp). Globbing patterns (also known as standard wildcards) allow us to include all the files within a particular folder. Using this technique relinquishes control of the file import order, but if we're writing styles in the SMACSS way this shouldn't be an issue.  

The current main SASS file with globbing:

```
@import 'base/bootstrap_overrides';
@import 'vendor/**/*';
@import 'base/**/*';
@import 'layout/**/*';
@import 'module/**/*';
@import 'state/**/*';
```

We first load the file that overrides Bootstrap's SASS variables. Overriding Bootstrap's SASS variables as opposed to overriding Bootstrap's compiled CSS allow us to easily make broad changes to colors, fonts, etc.

The next five imports use the `**/*` globbing pattern to include all the files within the corresponding folder.  

## Globbing & Gulp

Both SlimDocs Theme and Fabricator, the pattern library framework we're using for this project, use Gulp to compile SASS. Out of the box, Fabricator's Gulp setup doesn't include globbing. In order for the globbing to work for both the theme's and pattern library's Gulp setup, we needed to add the Gulp plugin [Gulp Sass Glob](https://www.npmjs.com/package/gulp-sass-glob).  

Here's a simplified Gulp task using the globbing plugin:

```
gulp.src(sass_dir + "toolkit.scss")
    .pipe(sassGlob())
    .pipe(sass())
    .pipe(autoprefixer())
    .pipe(minifyCss())
    .pipe(sourcemaps.write()) 
    .pipe(gulp.dest(css_dir));
```

The task starts with piping the main SASS file, in this case `toolkit.scss`, to the globbing plugin. The globbing plugin uses some regular expression magic to retrieve all the files specified in `toolkit.scss`. Then, Gulp proceeds with it's normal SASS processing tasks -- compiling the SASS, auto-prefixing the CSS,  minifying the CSS, adding a source map, and, finally, saving the new CSS file.