+++
title = "OrgRoam to Hugo"
draft = false
+++

## Installation of Ox-Hugo {#installation-of-ox-hugo}

You can follow the instructions on the original [documentation](https://ox-hugo.scripter.co/doc/installation/). For Spacemacs, under `dotspacemacs-configuration-layers`, you can add the following:

```elisp
(setq-default dotpacemacs-configuration-layers
   '(
     ...
     (org :variables
            org-enable-roam-support t
            org-enable-roam-ui t

            ;; Add the following line
            org-enable-hugo-support t   ; Enable Ox-Hugo
     )
     ...
    )
)
```


## Setting Up Ox-Hugo {#setting-up-ox-hugo}

You then need to set the variables `org-hugo-base-dir` to where your Hugo directory/repository is located, and `org-hugo-default-section-directory` to the category you'd like it to be (e.g. `posts` or `blog`). You can set it on the same variables section

```elisp
(setq-default dotpacemacs-configuration-layers
   '(
     ...
     (org :variables
            org-enable-roam-support t
            org-enable-roam-ui t
            org-enable-hugo-support t

            ;; Setting the variable names:
            org-hugo-base-dir "~/gitrepos/repo_name"
            org-hugo-default-section-directory "posts"
     )
     ...
    )
)
```

Here, since you've setup the `org-hugo-default-section-directory` to `posts`, the variable `{HUGO_SECTION}` will be `posts`. And since you've set `org-hugo-base-dir` to `~/gitrepos/repo_name`, then all the posts will be saved to `~/gitrepos/repo_name/{HUGO_SECTION}`.

This will also ask for a folder `~/gitrepos/repo_name/static` to be created. Make sure you create it. You can add it to your `.gitignore` file.

This can be paired with the [Hugo Deployment]({{< relref "hugo_deployment.md" >}}) to ensure it is properly deployed.