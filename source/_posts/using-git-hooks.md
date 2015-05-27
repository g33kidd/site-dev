title: Using Git Hooks to Deploy to Your Server
date: 2015-05-27 15:09:19
comments: true
tags:
- git
---
Using git hooks can be an overwhelming task to setup if you have no clue what to do. In this article I'll be taking you through the steps on how to setup your server with a simple post-receive hook.

### What are Git Hooks? ###
Git hooks allow you to fire custom scripts when certain actions occur. There are client-side and server-side hooks. We're only going to look at server-side hooks. There are a number of [hooks](http://git-scm.com/docs/githooks "Git Hooks Documentation") available, which all give a very detailed description of what it does and when it occurs.

---

### Setting Up The Server ###

First things first, make sure git is installed on the server.

``` bash
sudo apt-get update
sudo apt-get install git
```

Once git is installed on your server, go to any location on your server like **/home/user/** or any other location you wish to store your git repo(s). You want to create that directory and initialize a bare git repo from there.

``` bash
mkdir -p /home/user/project # create the directory
cd /home/user/project       # set current directory
git init --bare             # initialize the bare repository
```

Your folder structure will now look something like this:

``` bash
ls
branches  config  description  HEAD  hooks  index  info  objects  refs
```

### Create the Hook ###
Hooks live inside the **hooks/** directory within the repo you just created. You may create a bash file for any one of the hooks listed in the git hooks documentation linked above. Today, we will only be using the **post-receive** hook.

Create your post-receive hook and use nano to edit the file.
``` bash
sudo nano hooks/post-receive
```

This first example will basically move your files to the work-tree. Without anything extra, this is a very basic example.

``` bash
#!/bin/bash
git --work-tree=/var/www/project/html --git-dir=/home/user/project checkout -f
```

This example is more complex, but it will ultimately check if the branch pushed is the master branch. This is helpful if you have a team of developers and want to control which branches are allowed on different directories or repositories on the server.

``` bash
#!/bin/bash
while read oldrev newrev ref
do
   if [[ $ref =~ .*/master$ ]];
   then
        echo "Deploying master branch to production..."
        git --work-tree=/var/www/dev.g33kidd.com/html --git-dir=/root/github checkout -f
   else
        echo "$ref received. Only the master branch may be deployed to this server."
   fi
done
```

Although, you don't need to use bash in order to get your post-receive hook up and running, you may use Ruby as well if you are more comfortable with that.

There are some more examples and documentation on hooks [here](https://git-scm.com/book/es/v2/Customizing-Git-Git-Hooks "Customizing Git").

### How to Use ###
In your **local** git repo you can add your server repository you just created as a remote.
``` bash
git remote add production user@host_or_ip:project
```

Just a simple explanation of what is happening here. You created a bare repository on your server, when you push to your server, git will recognize which repo you have just pushed to. In turn it will execute your post-receive hook.

- **user**: the user on your server that has access to both --work-tree and --git-dir.
- **host_or_ip**: the host your are setting up the hook on.
- **project**: the name of the directory you created the repository in.


### Done ###
If you made it this far, congrats! And good luck discovering more options with git hooks to be more productive.

Feel free to share this article if you found it helpful. Leave a comment if you're stuck or if you spot an issue in my code.
