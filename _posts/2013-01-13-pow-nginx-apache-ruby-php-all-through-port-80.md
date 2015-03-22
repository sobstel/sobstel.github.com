---
title: "Nginx, Apache, Ruby, PHP all through port 80 at Mac OS X"
---

### Homebrew

[Homebrew](http://mxcl.github.com/homebrew/) is a package manager for OS X. It just makes things easier.

If you don't have it already:

    ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"

### Nginx

    brew install nginx

Edit `/usr/local/etc/nginx/nginx.conf` file to set `listen` to `8081` port (`server` section).

### Apache

    brew tap djl/homebrew-apache2
    brew install djl/apache2/apache24

Create (link) apache configuration in main brew etc directory: <kbd>ln -s /usr/local/Cellar/apache24/2.4.3/conf /usr/local/etc/apache2</kbd> (note to use correct version of apache2).

Edit <kbd>/usr/local/etc/apache2/httpd.conf</kbd> file to set <code>Listen</code> to <code>8082</code> port.

OS X comes with Apache pre-installed, so we need to make sure ours 2.4 is used, and not default 2.2 one.

Turn off "Web Sharing" in System Preferences (Sharing).

Make sure to have <code>/usr/local/bin</code> before <code>/usr/sbin</code> (where default Apache is installed), eg. add to .zshrc <kbd>export PATH=/usr/local/bin:$PATH</kbd>.



### Ruby

    brew install ruby

### PHP

    brew tap homebrew/dupes
    brew tap josegonzalez/homebrew-php
    brew options php54 # to check installation options available
    brew install php54 --with-fpm

Browse <a href="https://github.com/josegonzalez/homebrew-php/tree/master/Formula">josegonzalez/homebrew-php</a> for more PHP formulas, eg. <kbd>brew install php54-xdebug</kbd>.

### PHP Apache vhosts

Make sure you include the proxy_fcgi module in your httpd.conf so we can use its features:

    LoadModule proxy_module modules/mod_proxy.so
    LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so

Edit the configuration for a vhost of your choice, and add the following line to it:

    ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/path/to/your/documentroot/$1

### Pow

    curl get.pow.cx | sh

[Pow](http://pow.cx/) is a zero-config server for OS X. 

It's working at port 80 by default, but its port proxying feature lets you proxy virtual hosts to other ports on your computer (so we can proxy nginx apps to `8081` and apache apps to `8082`). 

You just create a file in ~/.pow with the virtual host as the filename and the port number as its contents, eg. `echo 8081 > ~/.pow/myapp`.

### Additional notes

Make sure to have all services added to <kbd>~/Library/LaunchAgents</kbd> directory and loaded via <kbd>launchctl load -w ...</kbd>. All instructions are provided after given package is installed with brew.
