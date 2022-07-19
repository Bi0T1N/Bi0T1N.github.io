---
layout: default
title:  "Preview GitHub Pages content locally with Docker and Visual Studio Code"
categories: [ruby, jekyll, github pages, visual studio code, docker, remote development]
---

# {{ page.title }}

## Introduction

In case you need to develop software that has dependencies on specific library versions other than your operating system provides, want to create a portable and easily reproducible setup of your development environment or test a written software easily against different versions of software or system libraries you should consider using Docker. Docker containers are isolated from the local operating system and thus are bundled with their own software, libraries and configuration files.
The very popular Visual Studio Code (VS Code) provides syntax highlighting, intelligent code completion, support for debugging and allows to install extensions which brings additional functionality. Besides that it supports also many user customization like specifying keyboard shortctus, preferences or using different themes.

![Overview of Visual Studio Code](/images/vs_code_overview.png)

## Visual Studio Code Remote Development

With the Visual Studio Code Remote Development extension one can develop within the Windows Subsystem for Linux, docker containers or over SSH on remote machines.
Here we'll focus on the Visual Studio Code Remote - Containers extension as it allows to use docker containers for the development within VS Code. Therefore the local source folder gets mounted into the container where all the build tools are installed. Thus it even allows to develop on machines that only run docker (or to develop from the web browser).
Notice that you could also copy your source files or clone a repository to the docker container but here I'll only consider mounting local files.

## Setup

### General

1. Install Visual Studio Code
2. Install Docker
3. Install Visual Studio Code Remote - Containers extension in VS Code:  
    Press Crtl+P, paste `ext install ms-vscode-remote.remote-containers` and hit enter
4. Install other useful VS Code Extension

## Dockerfile

The first step is do create (or re-use) a Dockerfile that specifies your development environment. The [GitHub Pages documentation](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll) describes the prerequisites and [dependencies](https://pages.github.com/versions/) for GitHub Pages. This information is contained in the following Dockerfile:

{% highlight docker %}
# Create a GitHub Pages container with the required Jekyll dependencies from a Ruby Alpine image

# Currently GitHub requires Ruby 2.7.x, see https://pages.github.com/versions/
FROM ruby:2.7-alpine

# Add Jekyll dependencies to Alpine Ruby image
RUN apk --no-cache add build-base gcc

# Update dependency manager and install GitHub Pages and its dependencies
RUN gem update bundler && gem install github-pages

WORKDIR /var/www/

EXPOSE 4000

CMD [ "jekyll", "serve", "--livereload", "--force_polling", "-H", "0.0.0.0", "-P", "4000" ]

# usage without Visual Studio Code:
# docker run -it --rm -v "$PWD":/var/www -p "4000:4000" <image_name>
{% endhighlight %}

Additionally it defines that the website content inside the Docker container should be put into `/var/www/`, that it exposes port `4000` for communicating with the HTTP server and that the default executing command is to start Jekyll. The `--livereload` flag adds some JavaScript to your page so that you don't need to refresh the page manually while doing changes. It refreshes it automatically when you save an edited file but you need also need to expose port `35729` (but I'm not sure if it'll always be this port so I left it out).

### Command Line

The required command line is already given as a comment in the Dockerfile above. The <image_name> variable is your local Docker image that can be determined by running `docker images` which should yield something like `vsc-bi0t1n.github.io-<hash>`.

```bash
docker run -it --rm -v "$PWD":/var/www -p "4000:4000" <image_name>
```

### Visual Studo Code

Using remote development with Visual Studio Code requires a `.devcontainer` folder with a `devcontainer.json` file that contains the instructions for your development environment. The file below provides the configuration for Jekyll with enabled live reload since it doesn't need to rebuild the Docker image if the port changed and you adapt it in the `devcontainer.json`.

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

    // IDs of extensions that are installed when the container is created
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
[Alpine Docker Image for GitHub Pages and Jekyll powered sites](https://github.com/Starefossen/docker-github-pages)  
