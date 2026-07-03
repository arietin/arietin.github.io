---
layout: page
title: How I Built This
---

# Setting Up My Site

A step by step guide for setting up a bare-bones version of this site, as of 2026-07-01.

## My System:
- OS: Windows 11
- Text Editor: VSCode
- Version Control: GitHub
- Host: GitHub

## Downloads:

### Ruby 3.1 with DevKit (newer versions caused compatibility issues)

1. Open PowerShell
2. Type in `winget search ruby`
3. Copy the id corresponding to `Ruby 3.1 with MSYS2` (for me it was `RubyInstallerTeam.RubyWithDevKit.3.1`)
4. To download, type `winget install [the copied id from step 3]`
   
### Jekyll (with Bundler)

1. With Ruby installed, open Command Prompt
2. Type `gem install jekyll bundler`

### Poole

1. Download and extract the files from the [Poole GitHub](https://github.com/poole/poole)

**NOTE:** Extract these files to a temporary location, NOT that of your webpage

### Hyde

1. Download and extract the files from the [Hyde GitHub](https://github.com/poole/hyde)

**NOTE:** These files can be extracted directly to where you want to build your WebPage

## Setting Up a Local Version

### Transfer Necessary Poole Files

1. Copy `Gemfile` and `Gemfile.lock` from `poole-master` to the top-level hyde folder
2. Copy the contents **from the line `# Gems` onward** (line 9 for me) of the `_config.yml` from `poole-master` to the `_config.yml` in the top-level hyde folder

### Make Necessary Edits 

#### To the `_config.yml` File:

1. Change the markdown parser: `markdown: redcarpet` &rarr; `markdown: kramdown`
2. Set `url` to `"localhost:8000"` (under the "#Setup" section)
3. Set `baseurl` to `"/_site/"` (under the "#Setup" section)
4. Delete the line `relative_permalinks: true` (no longer supported)

#### To `_includes/head.html`:
1. Replace all instances of `{{ site.baseurl }}` with `{{ site.url }}{{ site.baseurl }}` (relative links seem to not work, so they need to be explicitly set)

#### To `_includes/sidebar.html`:
1. Replace `{{ node.url }}` with `{{ site.url }}{{ site.baseurl }}{{ node.url }}`

### Run

1. Change directory to the topmost level of your hyde folder
2. Execute `bundle update`
3. Execute `bundle exec jekyll serve`

## Customize It

### Jekyll 101: What is it Doing?
1. It takes all the non-excluded `.md` (markdown) files and creates a folder for each in the `_site` folder. The markdown is used to populate an html template determined by the markdown file's header (e.g. with `layout: page` in the header, the `page.html` layout would be used). Templates are stored in the `_layouts` folder. 
2. A similar process is executed for every markdown file in the `_posts` folder. The main difference, however, is that these are grouped into folders based on their titles, which are expected to begin with dates in a YYYY-MM-DD format, such as `1572-11-5-a-new-star.md` (month and date can be single digit numbers). These dates determine the order posts are shown in on the homepage.
3. Posts are loaded in, with the number per page set by the `paginate: ` option in the `_config.yml` file.

### Changing Appearances

Change the color scheme by adding `class="theme-base-##"` to the body tag of the `_layouts/default.html` page. More details about which colors and numbers are available can be found on the [hyde GitHub page](https://github.com/poole/hyde#themes). Other aesthetic changes can be found there as well.

Icons are stored in the `public` folder. The easiest way to maintain compatibility when swapping out images is to keep the filenames.

Features of the sidebar are largely controlled by the `_config.yml` file, and the exact variables are shown in the `_includes/sidebar.html` file.

### Adding Pages

Pages can be added to the sidebar by creating a new `.md` file in the top level of the directory. The filename can be any valid string, but the head of this file should be:
```
---
layout: page
title: [Your Title Here]
---
```

Posts can be added to the site by creating a new `.md` file in the `_posts` directory. Each post filename should begin with a date, as [described above](#jekyll-101-what-is-it-doing).

## Host it on GitHub