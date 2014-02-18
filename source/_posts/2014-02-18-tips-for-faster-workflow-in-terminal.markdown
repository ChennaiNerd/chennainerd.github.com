---
layout: post
title: "Tips for fasten your workflow in terminal"
date: 2014-02-18 18:00:13 +0530
comments: true
categories: Bash
---

Without a doubt and question, Terminal is home for all programmers.
It always wonderful to have shortcuts to save our precious seconds.
Here we will discuss about techniques to fasten your workflow.
Place all `bash` function and aliases inside your `.bash_profile` or `.bashrc`.

### Create and Change Directory

Generally, we create a folder and then `cd` to created directory.
We can do it by

    take ()
    {
        mkdir -p $1 && cd $1
    }

Type `take foobar` will create and change directory to `foobar`

### Run static server

If you are a web developer, you wanted to view static files in browser.
Run your static server by

    server ()
    {
        local port="${1:-8000}";
        open "http://localhost:${port}/";
        python -c 'import SimpleHTTPServer;
        map = SimpleHTTPServer.SimpleHTTPRequestHandler.extensions_map;
        map[""] = "text/plain";
        for key, value in map.items():
            map[key] = value + ";charset=UTF-8";
        SimpleHTTPServer.test();' "$port"
    }

Type `server` insider any directory and visit `http://localhost:8000/` in browser

### Git aliases

If you are a git user, following aliases will save your time a lot.

    alias gcl='git clone'
    alias ga='git add .'
    alias gall='git add -A'
    alias gst='git status'
    alias gc='git commit -v'
    alias gco='git checkout'
    alias gl='git pull'
    alias gp='git push'
    alias gpo='git push origin'

### Run previous command

`!!` represents the previous command. Most of the time, we forgot to type sudo for
some command.

    sudo !!

Above will run the previous command with `sudo`. CAUTION: Your previous command MUST NOT BE
 `rm -rf /` :).

### Go back to old directory

Sometimes we want to change directory to previous working directory which we changed from. And revert to previous working directory we can use same command. This is vary much useful cd command.

    cd -


We know, there are lot of cool tips out there. Share with us what you got.
Happy hacking.

