---
layout: post
title: Install gitolite
---

After installation of Ubuntu 14.04 LTS, copy your local `id_rsa.pub_` file to your server:

    local$ scp ~/.ssh/id_rsa.pub git@server:~/yourname.pub
    root@server$ apt-get install git
    root@server$ adduser git
    root@server$ mv yourname.pub /home/git/yourname.pub
    root@server$ su git
    git@server$ mkdir -p ~/bin
    git@server$ git clone git://github.com/sitaramc/gitolite
    git@server$ gitolite/install -ln ~/bin
    add ~/bin to your PATH on server
    git@server$ gitolite setup -pk yourname.pub

That's it. Now, you can clone the repo (`git clone git@server:gitolite-admin`) and start configuring your git host.

More information on [http://gitolite.com/gitolite/index.html](http://gitolite.com/gitolite/index.html).
