---
layout: page
title: How I Built This
---

# Setting Up My Site

A step by step guide for setting up a bare-bones version of this site, as of 2026-08-08. To skip past the nitty-gritty and make a version for yourself, you can download a stripped down version of my site code here: [link goes here]().

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

*This will need to be reverted later if uploading to GitHub*

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

Hosting this site on GitHub required a number of additional steps, primarily to address deprecation (the latest updates to some of the poole themes are several years old) and GitHub requirements. I'm going to walk you through each change, file by file.

### index.html

#### frontmatter

Make sure the frontmatter (the first few lines of the file) reads: 

{% highlight ruby %}

---
layout: default
title: Home
pagination: 
  enabled: true
---

{% endhighlight %}

The key difference is that pagination is explicitly enabled on this page.

#### Post navigation buttons

Swap out the post navigation with the following code (it uses updated features afforded by a package called paginate-v2):

```html
{%raw%}
{% if paginator.total_pages > 1 %}
<div class="pagination">
  {% if paginator.next_page %}
    <a class="pagination-item older" href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Older</a>
  {% else %}
    <span class="pagination-item older">Older</span>
  {% endif %}
  {% if paginator.previous_page %}
    {% if paginator.page == 2 %}
      <a class="pagination-item newer" href="{{ site.url }}">Newer</a>
    {% else %}
      <a class="pagination-item newer" href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">Newer</a>
    {% endif %}
  {% else %}
    <span class="pagination-item newer">Newer</span>
  {% endif %}
</div>
{% endif %}
{% endraw %}
```
### Gemfile
Replace the line `gem 'jekyll-paginate'` with:

{% highlight ruby%}
group :jekyll_plugins do
  gem 'jekyll-paginate-v2', '>= 3.0'
end
{% endhighlight%}

Because this is not a package that GitHub natively has, we need to tell it to retrieve the package from a group called 'jekyll_plugins.'

Once this is done, I recommend executing `bundle update` to ensure the Gemfile.lock receives the corresponding changes.

### _config.yml

This is where the largest number of changes occurred. Firstly, I updated the following site variables to read:

{% highlight ruby %}
url:              "https://arietin.github.io"
baseurl:          ""
sourceprefix:     ""
{% endhighlight %}

Naturally, your website url would go in quotes on the url line.

I also replaced the old pagination options with the following settings, which I took from the Site configuration tab of the jekyll-paginate-v2 [documentation page on GitHub](https://github.com/sverrirs/jekyll-paginate-v2/blob/master/README-GENERATOR.md#site-configuration)

{% highlight markdown %}
############################################################
# Site configuration for the Jekyll 3 Pagination Gem
# The values here represent the defaults if nothing is set
pagination:
  
  # Site-wide kill switch, disabled here it doesn't run at all 
  enabled: true

  # Set to 'true' to enable pagination debugging. This can be enabled in the site config or only for individual pagination pages
  debug: false

  # The default document collection to paginate if nothing is specified ('posts' is default)
  collection: 'posts'

  # How many objects per paginated page, used to be `paginate` (default: 0, means all)
  per_page: 2

  # The permalink structure for the paginated pages (this can be any level deep)
  permalink: '/page/:num/' # Pages are index.html inside this folder (default)
  #permalink: '/page/:num.html' # Pages are simple html files 
  #permalink: '/page/:num' # Pages are html files, linked jekyll extensionless permalink style.

  # Optional the title format for the paginated pages (supports :title for original page title, :num for pagination page number, :max for total number of pages)
  title: ':title - page :num'

  # Limit how many pagenated pages to create (default: 0, means all)
  limit: 0
  
  # Optional, defines the field that the posts should be sorted on (omit to default to 'date')
  sort_field: 'date'

  # Optional, sorts the posts in reverse order (omit to default decending or sort_reverse: true)
  sort_reverse: true

  # Optional, the default category to use, omit or just leave this as 'posts' to get a backwards-compatible behavior (all posts)
  category: 'posts'

  # Optional, the default tag to use, omit to disable
  tag: ''

  # Optional, the default locale to use, omit to disable (depends on a field 'locale' to be specified in the posts, 
  # in reality this can be any value, suggested are the Microsoft locale-codes (e.g. en_US, en_GB) or simply the ISO-639 language code )
  locale: '' 

 # Optional,omit or set both before and after to zero to disable. 
 # Controls how the pagination trail for the paginated pages look like. 
  trail: 
    before: 2
    after: 2

  # Optional, the default file extension for generated pages (e.g html, json, xml).
  # Internally this is set to html by default
  extension: html

  # Optional, the default name of the index file for generated pages (e.g. 'index.html')
  # Without file extension
  indexpage: 'index'
{% endhighlight %}

Ensure that under the `plugins` tab is the line `  - jekyll-paginate-v2`

### _includes/head.html

Ensure that all the stylesheet and icon links are as follows:
`{{ site.url }}{{ site.baseurl }}/public/css/poole.css"`

### _includes/sidebar.html

Ensure that the `href` (link) for for nodes is `href="{{ node.url }}"` 

Also, replace instances of `{{ site.baseurl }}` with `{{ site.url }}`


### Upload to GitHub

1. Log into github or create a new account
2. Create a new repository titled '[your username].github.io' (e.g. `arietin.github.io`)
3. Upload all the files in the directory on your computer to this repository 
   1. *Alternatively, if you have GitHub desktop or are otherwise pushing files from your computer, transfer your files to the appropriate location and push*
4. Click on `Settings`. On the Left side of the screen, there should be a navigation menu with an item called `Pages`. Select it. 
5. Under `Build and Deployment` there should be a dropdown box near to the word **source**. Select `GitHub Actions`
6. On the navigation bar at the top of the screen, select `Actions`
7. From the menu on the left side of the screen, select the green `New workflow` button
8. Search for the  `Jekyll` workflow and select `configure `
   1. **NOTE: Do not select `GitHub Pages Jekyll` as it is a different workflow**
9. Click the green `commit changes` button. 
10. Ensure everything is pulled into the main branch. 
11. Under the actions page a workflow run should begin. Select it and click on the link to your website!

With your site hosted, you should be able to make all the necessary changes you'd like, and they will automatically be taken up as you push them to the server (add them to the repository).

At this point, I cleaned up the site, removing redundancies and other features I was not interested in.