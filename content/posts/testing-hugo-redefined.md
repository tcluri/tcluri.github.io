+++
title = "Testing Hugo Redefined and Local Build"
author = ["Tejaa Chintaluri"]
description = "Testing the end-to-end publishing workflow from Org Mode in Doom Emacs to Hugo Theme Redefined."
date = 2026-09-05T06:50:00+05:30
tags = ["hugo", "doomemacs", "blog"]
draft = false
+++

Welcome to the new publishing flow! This post verifies the end-to-end integration between Doom Emacs, `ox-hugo`, and the updated **hugo-theme-redefined**.

<!--more-->


## What's New? {#what-s-new}

We upgraded the theme to be fully compatible with **Hugo v0.164.0** and simplified our publishing pipeline to a **Local Build Architecture**.


## Sidenote Demonstration {#sidenote-demonstration}

Here is some text with an inline sidenote #+begin_sidenote
Sidenotes appear cleanly in the right margin on desktop screens and inline on mobile devices!
\#+end_sidenote to show how flexible Org shortcodes are.


## Code Highlighting {#code-highlighting}

Here is an example snippet highlighted with Chroma:

```elisp
;; Doom Emacs ox-hugo export command
(defun my/publish-post ()
  "Export the current Org subtree to Hugo markdown."
  (interactive)
  (org-hugo-export-wim-to-md))
```


## Conclusion {#conclusion}

Everything is configured and ready for smooth, distraction-free writing in Emacs!
