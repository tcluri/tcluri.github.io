+++
title = "Blog using org-mode in Doomemacs with Hugo"
author = ["tcluri"]
date = 2024-03-23T14:00:00+05:30
tags = ["blog", "org", "doomemacs", "hugo"]
draft = false
+++

Hello! This is a post describing how anyone who uses Doomemacs can setup a blog on Github user pages and get writing.
To get started, familiarize yourself with what I'm assuming you know to do.

-   Know how doomemacs works to the extent that you can find where your init.el is and upgrade/sync from the commandline.
-   Know how to install the Go programming language on your linux machine and can install a go package(😉 Hugo)
-   Know how to use git well enough to commit/push your changes to a repository online - we'll be using Github
-   Know how to do a backflip once we are done, in your head is fine too

If you don't know any of these, read on - there will be links you might find helpful. We are here to learn 😃


## Doomemacs {#doomemacs}

Doomemacs is a starter pack of default configurations for Emacs. You get many things out of the box -  which you can disable/enable away at your will.
Try to use the latest doom configuration, I tend to upgrade doomemacs once in 3 months or whenever it is necessary which usually means that a package got an upgrade or a bug that got fixed.
Once you try it, there is no going back to other configs/setups unless you are the person who loves your current Emacs config 🫡

Quick refresher of doomemacs commands, from the commandline:

-   `doom upgrade` - upgrades doom itself, fetching latest packages and their configuration.
-   `doom sync` - installs/removes packages and loads the packages by reading your doom config.


### Enable org-mode and +hugo it {#enable-org-mode-and-plus-hugo-it}

In doomemacs, org-mode should be enabled by default. To add `ox-hugo`, go to your `init.el` file in your doom config folder. You can use `SPC f p` and then select by filename.

Under the `:lang` heading, look for the line with `org` and add `+hugo` like below

```emacs-lisp
  :lang
  ;; ... OTHER packages
  ;;nix               ; I hereby declare "nix geht mehr!"
  ;;ocaml             ; an objective camel
  (org +hugo)         ; organize your plain life in plain text
  ;;php               ; perl's insecure younger brother
  ;;plantuml          ; diagrams for confusing people more
```

If you are using standard Emacs config, an equivalent config with `use-package` is this

```emacs-lisp
(use-package ox-hugo
  :ensure t
  :pin melpa
  :after ox)
```

This enables [ox-hugo](https://ox-hugo.scripter.co/) package which exports your posts in your `.org` file to markdown that is readable by Hugo(which we will now install)


## Hugo {#hugo}

Hugo is a static site generator written in Go. Hugo is great and everyone should use it. Let's get it installed on our machine using commandline,
this way we get to use the latest version and upgrade later on if needed
{{% sidenote %}}
This depends on if the theme you choose also supports the latest Hugo version.
See under "Get Hugo"
{{% /sidenote %}} . But before we do that we need to have the Go programming language installed.


### Installing Golang {#installing-golang}

Go to the install [page](https://go.dev/doc/install) and follow the instructions, they are clear and make sure you have added `/usr/local/go/bin` to the PATH environment variable.
Confirm it is installed using `go version` in your terminal, the version should be `1.20` or greater since Hugo requires this.


### Get Hugo {#get-hugo}

To install Hugo after you have installed the Go language run the following command,

```nil
go install github.com/gohugoio/hugo@latest
```

This is the standard version of Hugo and not the extended edition, if you need that refer [here](https://github.com/gohugoio/hugo?tab=readme-ov-file#build-from-source)

🚨 Installing Hugo with `@latest` might not work well with your theme and can break your site.
To check if a theme works well with a Hugo version you have, head over to the theme's repository and look in the `config.toml` file.

An example `config.toml` file in a theme's repository, here the maximum supported Hugo version is `0.84.2`

```nil
[module]
  [module.hugoVersion]
    extended = true
    min = "0.55.0"
    max = "0.84.2"
```

By default, binaries are installed to the bin subdirectory of the default `GOPATH` (`$HOME/go` in linux) so make sure to add it like so in your `.bashrc`

```nil
# Add go installs to PATH
export PATH="$PATH:~/go/bin"
```

In a new terminal or once you have sourced your `.bashrc` file, check that hugo is installed using `hugo version`.


## Setting things up {#setting-things-up}

Create an empty git repository on Github, the name of your repository should be `<username>.github.io`
and clone it on your machine to a folder named `blog` or something else.

Now intialize hugo in the blog folder, use `--force` since it is a non-empty directory and it contains `.git` folder

```nil
hugo new site --force blog
```

From inside the blog folder, `cd` into it, make hugo blog as a hugo module. This enables the blog's theme to be used as a module.

```nil
hugo mod init github.com/username/username.github.io
```

Add the following lines to your `hugo.toml` (previously it was `config.toml`) to add a theme. Select any you like, there are [lots](https://themes.gohugo.io/).

```config
[module]
  [[module.imports]]
    path = "github.com/athul/archie"
```

On the command line, run the following

```nil
hugo mod get -u
```

From here on, you can follow from the step #5 from ox-hugo's quickstart i.e "Appending lines to the site config": <https://ox-hugo.scripter.co/doc/quick-start/>


### Writing a post and exporting it {#writing-a-post-and-exporting-it}

Once you are done writing your post in org file residing in your `content-org/` directory, export it using `C-c C-e H H`.
This will create a markdown file in `content/` directory with the name that you provide in the properties of the org heading

```org
:PROPERTIES:
:EXPORT_FILE_NAME: org-doomemacs-hugo
:END:
```

Hugo reads this markdown file in the `content/` directory and generates the necessary contents for the site.
Commit the markdown file to your repository.


### Auto deploy using Github actions {#auto-deploy-using-github-actions}

To setup github actions to start deploying once you commit a markdown post that gets generated in the `content/` directory to your repository on Github see [here](https://gohugo.io/hosting-and-deployment/hosting-on-github/#step-by-step-instructions).

-   Change the settings of your repository to enable Github actions
-   Create a file `.github/workflows/hugo.yaml` in your repository and paste the contents from the link
-   Commit this file to your repository and see the magic happen 🎉


## References {#references}

For references (or) further reading:

-   <https://ox-hugo.scripter.co/doc/quick-start/>
-   <https://ridaayed.com/posts/howto-ox-hugo-github-pages/>


## Thanks {#thanks}

Thanks to [Shantanu](https://baali.muse-amuse.in/) and [Punch](https://github.com/punchagan) for suggesting me to write this post and for feedback which helped clarify
things. Thank you for reading
