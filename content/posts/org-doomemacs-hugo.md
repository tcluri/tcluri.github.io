+++
title = "Blog using org-mode in Doomemacs with Hugo"
author = ["tcluri"]
date = 2024-03-23T14:00:00+05:30
tags = ["blog", "org", "doomemacs", "hugo"]
draft = false
+++

Hello! This is a post describing how anyone who uses Doomemacs can setup a blog and get writing.
To get started, familiarize yourself with what I'm assuming you know to do. If you don't, read on - there will be links you might find helpful. We are here to learn 😃

-   Know how doomemacs works to the extent that you can find where your init.el is and upgrade/sync from the commandline.
-   Know how to install the Go programming language on your linux machine and can install a go package(😉 Hugo)
-   Know how to use git well enough to commit/push your changes to a repository online - we'll be using Github
-   Know how to do a backflip once we are done, in your head is fine too


## Doomemacs {#doomemacs}

Doomemacs is a configuration framework for Emacs. You get many things out of the box - a starter pack of which you can disable/enable away options at your will.
Try to use the latest doom configuration, I tend to upgrade doomemacs once in 3 months or whenever it is necessary which usually means that a package got an upgrade or a bug that got fixed.
Once you try it, there is no going back to other configs/setups unless you are the person who loves your current Emacs config 🫡

Quick refresher of doomemacs commands, from the commandline:

-   `doom upgrade` - upgrades doom itself, fetching latest packages and their configuration.
-   `doom sync` - installs/removes packages and loads the packages by reading your doom config.


### Enable org-mode and +hugo it {#enable-org-mode-and-plus-hugo-it}

To install org-mode, go to your `init.el` file in your doom config folder. You can use `SPC f p` and then select by filename.

In it, enable org-mode and hugo under the `:lang` heading.

```emacs-lisp
       :lang
       ;; ... OTHER packages
       ;;nix               ; I hereby declare "nix geht mehr!"
       ;;ocaml             ; an objective camel
       (org +hugo)         ; organize your plain life in plain text
       ;;php               ; perl's insecure younger brother
       ;;plantuml          ; diagrams for confusing people more
```

This enables [ox-hugo](https://ox-hugo.scripter.co/) package which exports your posts in your `.org` file to markdown that is readable by Hugo(which we will install now).

If you are using standard Emacs config, this does quite a lot on the org side of things and you can see exactly what it is [here](https://github.com/doomemacs/doomemacs/tree/master/modules/lang/org).


## Hugo {#hugo}

Hugo is a static site generator written in Go. Hugo is great and everyone should use it. Let's get it installed on our machine using commandline,
this way we get to use the latest version and upgrade later on if needed. Before we do that we need to have the Go programming language installed.
Go to the install [page](https://go.dev/doc/install) and follow the instructions, they are clear and you can confirm it is installed using `go version` in your terminal, the
version should be `1.20` or greater since Hugo requires this.

To install Hugo after you have installed the Go language run the following command,

```nil
go install github.com/gohugoio/hugo@latest
```

This is the standard version of Hugo and not the extended edition, if you need that refer [here](https://github.com/gohugoio/hugo?tab=readme-ov-file#build-from-source)

You can check that hugo is installed using `hugo version` at the commandline.


## Setting things up {#setting-things-up}

Create an empty git repository on Github, the name of your repository should be `<username>.github.io`
and clone it on your machine to a folder named `blog` or something else.

Now intialize hugo in the blog folder, use `--force` since it is a non-empty directory since it contains `.git` folder

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


### Github actions {#github-actions}

To setup github actions to start deploying once you commit a post to your repository on Github i.e after writing your post in `content-org` directory and exporting it via `ox-hugo` using
`C-c C-e H H`, see [here](https://gohugo.io/hosting-and-deployment/hosting-on-github/#step-by-step-instructions).


## References {#references}

For references (or) further reading:

-   <https://ox-hugo.scripter.co/doc/quick-start/>
-   <https://ridaayed.com/posts/howto-ox-hugo-github-pages/>
