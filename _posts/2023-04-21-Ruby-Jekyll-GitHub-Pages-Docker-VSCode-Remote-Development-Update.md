---
layout: default
title:  "Preview GitHub Pages content locally with Docker and Visual Studio Code - Update"
categories: [ruby, jekyll, github pages, visual studio code, docker, remote development, debian]
---

# {{ page.title }}

## Introduction

This is a short update for the [older article about previewing GitHub Pages content locally with Docker and Visual Studio Code](https://bi0t1n.github.io/ruby/jekyll/github%20pages/visual%20studio%20code/docker/remote%20development/2022/07/19/Ruby-Jekyll-GitHub-Pages-Docker-VSCode-Remote-Development.html). It uses Debian Bullseye instead of Alpine Linux now.

## Changes

The updated `devcontainer.json` allows to customize for specific editors. It also supports [Dev Container Features](https://containers.dev/features) which is used here to add `git` on top of the image created with the `Dockerfile`.

### Dockerfile

The first step is to create (or re-use) the Dockerfile that specifies your development environment. The [GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll) describes the prerequisites and [dependencies](https://pages.github.com/versions/) for GitHub Pages. This information is contained in the following Dockerfile:

{% highlight docker %}
# Create a GitHub Pages container with the required Jekyll dependencies from a Ruby Alpine image

# Currently GitHub requires Ruby 2.7.x, see https://pages.github.com/versions/
FROM ruby:2.7-bullseye

# Update image
RUN apt update && apt upgrade

# Update dependency manager and install GitHub Pages and its dependencies
RUN gem update bundler && gem install github-pages

WORKDIR /var/www/

EXPOSE 4000

CMD [ "jekyll", "serve", "--livereload", "--force_polling", "-H", "0.0.0.0", "-P", "4000" ]

# usage without Visual Studio Code:
# docker run -it --rm -v "$PWD":/var/www -p "4000:4000" <image_name>
{% endhighlight %}

Additionally it defines that the website content inside the Docker container should be put into `/var/www/`, that it exposes port `4000` for communicating with the HTTP server and that the default executing command is to start Jekyll. The `--livereload` flag adds some JavaScript to your page so that you don't need to refresh the page manually while doing changes. It refreshes it automatically when you save an edited file but you need also need to expose port `35729` (but I'm not sure if it'll always be this port so I left it out).

### Visual Studo Code

Using remote development with Visual Studio Code requires a `.devcontainer` folder with a `devcontainer.json` file that contains the instructions for your development environment. The file below provides the configuration for Jekyll with enabled live reload since it doesn't need to rebuild the Docker image if the port changed, you simply adapt it in the `devcontainer.json` if required. Additionally it installs `git` thus it is possible to use source code management from within your editor.

{% highlight json %}
// For format details, see https://aka.ms/devcontainer.json. For config options, see the README at:
// https://github.com/microsoft/vscode-dev-containers/tree/v0.241.1/containers/alpine
{
	"name": "Jekyll for GitHub Pages",
	"build": {
		"dockerfile": "Dockerfile"
	},

	// Use 'forwardPorts' to make a list of ports inside the container available locally.
	"forwardPorts": [4000, 35729],

	// 'postAttachCommand' to run a command each time a tool has successfully attached to the container.
	"postAttachCommand": [ "jekyll", "serve", "--livereload", "--force_polling", "-H", "0.0.0.0", "-P", "4000" ],

    "customizations": {
        // Configure properties specific to VS Code.
        "vscode": {
            // IDs of extensions you want installed when the container is created.
            "extensions": [
                // jekyll and liquid templating syntax highlighting
                "sissel.shopify-liquid",
                // markdown
                "yzhang.markdown-all-in-one",
                "DavidAnson.vscode-markdownlint",
                // editing
                "streetsidesoftware.code-spell-checker",
            ]
        }
      },

    // A set of simple and reusable Features. Quickly add a language/tool/CLI to a development container.
    "features": {
        // Install an up-to-date version of Git.
        "ghcr.io/devcontainers/features/git:1": {
            "version": "os-provided"
        }
    }
}
{% endhighlight %}

## Conclusion

Several Dockerfiles and write-ups are available but none used the correct requirements as stated by GitHub Pages. They were written some time ago and are therefore outdated/no longer maintained.  
Thus we have here a new version that is current at the moment.

## Sources

[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)  
[Docker](https://www.docker.com/)  
[Remote - Containers - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)  
[GitHub Pages Documentation - GitHub Docs](https://docs.github.com/en/pages)  
[Jekyll • Simple, blog-aware, static sites | Transform your plain text into static websites and blogs](https://jekyllrb.com/)  
[VSCode, Docker, and Github Pages](https://www.allisonthackston.com/articles/vscode-docker-github-pages.html)  
[Debian “bullseye” Release Information](https://www.debian.org/releases/bullseye/)  
