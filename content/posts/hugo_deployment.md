+++
title = "Hugo Deployment"
tags = ["Github"]
draft = false
+++

## Setting Up a Hugo Directory {#setting-up-a-hugo-directory}


### Create a New Site {#create-a-new-site}

```bash
hugo new site repo_name
```


### Add a Theme {#add-a-theme}

Go inside the Hugo directory

```bash
cd repo_name
```

Make it into a Git repository, and then add the theme as a submodule

```bash
git init
git submodule add add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

Then add the theme to your `config.toml` file.

```yaml
# config.toml
baseURL = 'https://example.org'
languageCode = 'en-us'
...

# Add this line
theme = "ananke"
```


### Change the Base URL {#change-the-base-url}

In the case you are using this as a Projects Page, you might want to change the base URL.

```yaml
# config.toml
baseURL = "https://nicolasshu.com/repo_name"
```


### Add New Posts {#add-new-posts}

Now to add new posts, it should go under `content/{CATEGORY}/{POSTNAME}.md`. For example, you can create a new post

```markdown
<!-- content/post/my-post.md -->
---
title: "My Post"
date: 2019-03-26T08:47:11+01:00
draft: false
---
```

Here, `draft` should be `false` if you'd like it published, or `true` if you'd like it not published.


### Preview the Hugo Website {#preview-the-hugo-website}

To run it, simply go to the root (i.e. `repo_name/`), then run

```bash
hugo server -D
```

It is possible that you may not be able to visualize anything. It may return the following error:

```bash
WARN 2022/09/03 20:51:06 found no layout file for "HTML" for "home": You should create a template file which matches Hugo Layouts Lookup Rules for this combination.
```

This may be because you might need to re-download the submodule (i.e. theme; [Source](https://stackoverflow.com/a/65745209)). So you can run

```bash
git submodule init
git submodule update
```

Then run `hugo server -D` again, and it should work!


## Deploying to Github Pages {#deploying-to-github-pages}


### Add to an Empty Repository {#add-to-an-empty-repository}

From this point, you may run `git init`, but you may have already run it in the beginning. Then, you may `git add` all of your current files. Once you're done with that, you can run:

```nil
git commit -m "Initial commmit"
git push
git remote add origin git@github.com:nicolasshu/repo_name.git
git push origin
git push --set-upstream origin main
```


### Setup Github Actions {#setup-github-actions}

[Source](https://gohugo.io/hosting-and-deployment/hosting-on-github/). Now, create a file on `repo_names/.github/workflows/gh-pages.yml`, and add the following:

```yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Now add this to your repository. If you navigate to the `Actions` tab on your repository, you'll see a series of different new Github Actions. Additionally, you will see a new branch called `gh-pages`.


### Deploy the Page {#deploy-the-page}

Now go to your `Settings` &gt; `Code and automation` &gt; `Pages`. Now set the following:

-   Source: `Deploy from a branch`
-   Branch: `gh-pages` `/ (root)`

Then press `Save`.

{{< figure src="/ox-hugo/hugo_deployment-github_pages.png" >}}

After a few minutes, you should see your website at `https://nicolasshu.com/repo_name`