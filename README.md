# Strymon Source Code Website

This website is built with [Jekyll](https://jekyllrb.com/) and the
[Type Theme](https://github.com/rohanchandra/type-theme), refer to their
documentation for details.

To build this website locally, it is recommended to use Ruby with [Bundler](http://bundler.io/):

    # make sure to check out any submodules
    git submodule update --init
    # install the Ruby dependencies locally
    bundle install --path vendor/bundle
    # build and serve the website locally
    bundle exec jekyll serve


On GitHub Pages, the site is automatically built for each pushed commit. Please consult [GitHub Help](https://help.github.com/categories/github-pages-basics/) for additional information.

Blog posts are stored in the `_posts` folder, the documentation pages are in the folder `_docs`. 

### Creating a new blog post

Creating a new blog post is as easy as creating a new correctly dated file in the `_posts` folder. For example, create a blog post with the current date in the filename `_posts/2018-09-12-strymon-0.5.0-released.md` and the following content:

    ---
    layout: post
    title: Strymon 0.5.0 Released
    ---

    Your release announcement text here.


If your file ends in `.md`, you should use Markdown for text formatting. You can however also choose to use HTML by creating an `.html` file instead.

Once pushed to the `strymon-system.github.io` repository, it will show up automatically after some time, assuming the date of the post is not in the future. If the date is in the future, it will not show up!

Please read up on [how to write a blog post in Jekyll](https://jekyllrb.com/docs/posts/) for more details.

### Adding documentation pages

Documentation pages are rendered for any files Markdown files (with a YAML front matter) in the `_docs` folder. A typical YAML front matter might look like this:

    ---
    title: Title of the Documentation Page
    nav-index: 4
    category: Category Name
    ---

The `title` attribute is mandatory and rendered prominently at the top of each document and used as the link title in the navigation menu.

Note that any category referred to in the `category` attribute must also be defined in the `docs_categories` collection in `_config.yml`. Otherwise it will be put into the default category at the top of the documentation menu.

The `nav-index` of each document defines the order of the document within its category. Documents without any `nav-index` will be put at the bottom.

Some documentation pages (e.g. the `reconstruction` docs) are kept in their own repository, but added to the main website repository as a submodule. 

In include documents from other repositories into the main page, you will need to:
  1. create (or update) the git submodule for the remote repository in the [`submodules`](https://github.com/strymon-system/strymon-system.github.io/tree/master/submodules) folder
  2. create a symbolic link from the `_docs` folder into your submodule. E.g.: `_docs/sessionization/concepts.md` → `../submodules/reconstruction/docs/concepts.md` and add it to git.

The GitHub Pages runner will automatically checkout the submodule and resolve the symlink on the final build.
