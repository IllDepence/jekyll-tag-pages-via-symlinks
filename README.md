### Plugin-free tag pages via symlinks in Jekyll

Minimal PoC demonstrating how symlinks can be used to generate per tag pages in Jekyll.

#### How does this work?

The directory `tag/` contains a file `_tag_by_fn.md` as well as symlinks to that file. Each symlink is named according to a tag. E.g.:

```
$ grep bar _posts/*md
_posts/2021-01-04-four.md:tags: bar one
_posts/2021-01-05-five.md:tags: bar two

$ file tag/bar.md
tag/bar.md: symbolic link to _tag_by_fn.md
```

In `_tag_by_fn.md` the variable `page.name` is accessed to determine the set of posts to output.

```
{% assign output_tag = page.name | remove:'.md' %}
```

When Jekyll processes symlinks, `page.name` refers to the name of the *symlink*, not the name of the *target*. Example: when `tag/bar.md` is processed the contents of `_tag_by_fn.md` are read, but the value of `page.name` is `bar.md`. Therefore, a file `tag/bar.html` will be generated linking all posts tagged `bar` (i.e., posts four and five).

#### Automatically generating symlinks

You can run the following oneliner to automatically generate one symlink for each tag that appears in your posts.

```
$ grep -Poh '(?<=tags:).*$' _posts/*.md | sed -e 's/\s\+/\n/g' | sort | uniq | xargs -I{} ln -s _tag_by_fn.md tag/{}.md
```
