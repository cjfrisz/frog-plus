# Frog+

A script for use with Greg Hendershott's
[Frog](https://github.com/greghendershott/frog) that streamlines
some common workflows for writing blog posts, building/previewing
the associated website, and deploying it to a remote hosting server.

As do many quick-and-dirty workarounds, Frog+ started life as a
10-line shell script and quickly grew, so it may migrate to a more
fully-featured language, i.e. Racket. It is likely that several of
Frog+'s features show up in Frog itself eventually, at which point
they will likely be deprecated in Frog+.

If you choose to use this script, please **be careful**. Little/no
work has been done to guarantee compatibility with different (or even
standard) Frog directory setups. I'm sharing it simply because I found
it useful for my own needs, and thought others might too.

Written and tested on Mac OS X 10.9.1 (Mavericks). Linux compatibility
planned, but not currently implemented


### Installation

Simply put the `frog-plus` script into your path.

Assumes that Frog is already installed and configured (which itself
requires [Racket](http://racket-lang.org)). Also uses the Z shell
(zsh) and some utilities/options that may only work on Mac.

For best results, open `frog-plus` in your editor of choice and set the
values for the following variables:

 * `host` -- Host name of the web server to which you're deploying
 * `wesite_path` -- Path on the host to which the website will be
   deployed
 * `blog_dir` -- Output directory used by Frog. This should match the
   value of "output-directory" in your `.frogrc`


### Documentation

Frog+ provides several subcommands, and can be invoked with one or
more of them at once. When specifying multiple subcommands, each
one is executed in the order specified. For example, `frog-plus clean
build` first performs a Frog clean (`raco frog --clean`) and then a
build (`raco frog --build`). The full list of subcommands (with
explanations) is as follows:

 * new "\<post title\>"

Creates a new post with the given title (via `raco frog --new "<post
title>"`) and opens the file in the editor given by the environment's
`EDITOR` variable.

The `new` command also creates a symlink to the new post file (named
.draft) in the directory where the command was invoked. See also
`resume` and `delete-draft`.

  * build

Builds the website. This is equivalent to `raco frog -b`.

  * preview

Preview the website. This is equivalent to `raco frog -p`.

Note that if the `fixup` subcommand has been used before `preview`,
the preview will likely not represent what the deployed website
will look like.

  * fixup

Fixes up references to css and js files, as well as to contents of the
generated "feeds" and "tags" directories when using an output directory
other than ".". It also copies the "css," "img," and "js" directories
from top-level into the output directory so that the directory may
be used as a self-contained section of a website. 

An example of the intended use case for `fixup` would be if you
have an existing website, www.example.com, and you want to use Frog only
for the blog portion, www.example.com/blog. To do so, you would set
"output-directory" in your `.frogrc` and the `blog_dir` variable in
`frog-plus` to "blog," run `build` and `fixup`, then copy the output
"blog" directory to the host server (i.e. use the `deploy` subcommand).

  * deploy

Uses the `rsync` command to copy the output directory (set in `.frogrc`)
to the website host. Requires setting the `host` and `website_path`
variables at the top of the `frog-plus` script file.

  * clean

Removes all generated files. If using "." for the output directory, this
executes `raco frog -c` and removes any non-front-page index.html files
(i.e. index-[0-9]+.html) as well as the sitemap.txt file.

If using an output directory other than than ".", that directory is
simply deleted.

  * resume

Opens the `.md` file for the most recently created page using the editor
specified by the `EDITOR` environment variable.

  * delete-draft

Deletes the `.md` file for the most recently created page and the
`.draft` symlink.

  * init

Initializes the current directory as a Frog project. Equivalent to `raco
frog --init`.

  * all

Shorthand for `frog-plus build fixup`.

  * all-deploy

Shorthand for `frog-plus build fixup deploy`.

- - -

All other inputs are passed to frog unmodified.


### Pull requests

Pull requests are welcome, though your time may be better spent (as
might mine) by working on Frog itself.


### License

Copyright © 2013 Chris Frisz (see included LICENSE file).
