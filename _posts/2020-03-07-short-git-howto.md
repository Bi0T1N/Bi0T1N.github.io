---
layout: default
title:  "Short Git HowTo"
categories: [git, setup, howto]
---

# {{ page.title }}

## Installation
Download latest [git](https://git-scm.com/) release and install.

## Configuration
1. set global username for commits
	- `git config --global user.name "Bi0T1N"`
2. set global e-mail address for commits (hiding private e-mail)
	- `git config --global user.email "Bi0T1N@users.noreply.github.com"`
3. generate SSH key
	- `ssh-keygen -t rsa -b 4096 -C "Bi0T1N@users.noreply.github.com"`
4. verfiy ssh agent is running
	- `eval $(ssh-agent -s)`
5. add ssh key to agent
	- `ssh-add ~/.ssh/<id_rsa name>`
6. copy ssh key content to clipboard to add it on github website
	- `clip < ~/.ssh/<id_rsa.pub file>`
7. load ssh key always when starting git console
	- `nano ~/.ssh/config`
	- add following code to it
      ```
      Host github.com
      IdentityFile ~/.ssh/<id_rsa name>
      ```
8. test ssh connection
	- `ssh -T git@github.com`

## GitHub workflow via terminal through [hub](https://hub.github.com/)
1. download [hub](https://hub.github.com/) release
2. extract everything <somewhere>
3. add directory with hub.exe to your path environment
	- Windows -> System -> Advanced Settings -> Environment Variables
	- add your <somewhere> path
4. enjoy

**Example workflow**:
- #### Example workflow for contributing to a project:
    ```
    $ hub clone github/hub
    $ cd hub
    ```
- #### create a topic branch
    ```
    $ git checkout -b feature
    ```
- #### make some changes...
    ```
    $ git commit -m "done with feature"
    ```
- #### It's time to fork the repo!
    ```
    $ hub fork --remote-name=origin
    -> (forking repo on GitHub...)
    -> git remote add origin git@github.com:YOUR_USER/hub.git
    ```
- #### push the changes to your new remote
    ```
    $ git push origin feature
    ```
- #### open a pull request for the topic branch you've just pushed
    ```
    $ hub pull-request
    -> (opens a text editor for your pull request message)
    ```

**NOTE**: After forking, you only need to repeat the last two steps for new contributions.

## Add new functions to git/cmd.exe
Add executable to your $PATH variable:  
  - Windows -> System -> Advanced Settings -> Environment Variables  
  - add the path to the executable
