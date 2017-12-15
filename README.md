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


On GitHub Pages, the site is automatically built for each push. Please consult
[GitHub Help](https://help.github.com/categories/github-pages-basics/) for
additional information.

## Adding Documentation Pages

Documentation pages are rendered for any files Markdown files (with a YAML
front matter) in the `_docs` folder. A typical YAML front matter might look
like this:

    ---
    title: Title of the Documentation Page
    nav-index: 4
    category: Category Name
    ---

The `title` attribute is mandatory and rendered prominently at the top of each
document and used as the link title in the navigation menu.

Note that any category referred to in the `category` attribute must also be
defined in the `docs_categories` collection in `_config.yml`. Otherwise it
will be put into the default category at the top of the documentation menu.

The `nav-index` of each document defines the order of the document within
its category. Documents without any `nav-index` will be put at the
bottom.
