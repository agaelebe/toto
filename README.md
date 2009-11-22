toto
====

the tiniest blogging engine in Oz!

introduction
------------

toto is a git-powered, minimalist blog engine for the hackers of Oz. The engine weights around ~200 sloc at its worse.
There is no toto client, at least for now; everything goes through git. The implications are:

philosophy
----------

Everything that can be done better with another tool should be, but one should not have too much pie to stay fit.

how it works
------------

- content is entirely managed trough **git**; you get full fledged version control for free.
- articles are stored as _.txt_ files, with embeded metadata (in yaml format).
- templating is done through **ERB**.
- toto is built right on top of **Rack**.
- comments are handled by disqus:(http://disqus.com)
- individual articles can be accessed through urls such as _/2009/11/21/blogging-with-toto_
- the archives can be accessed by year, month or day, wih the same format as above.

synopsis
--------

One would start by forking or cloning the toto-skeleton repo, to get a basic skeleton:

    $ git clone git://github.com/cloudhead/toto-skeleton.git

One would then edit the template at will, it has the following structure:

    templates/
    |
    +- layout.rhtml      # the main site layout, shared by all pages
    |
    +- feed.builder      # the builder template for the atom feed
    |
    +- pages/            # pages, such as home, about, etc go here
       |
       +- index.rhtml    # the default page loaded from `/`, it displays the list of articles
       |
       +- article.rhtml  # the article (post) partial and page
       |
       +- about.rhtml

One could then create a .txt article file in the `articles/` folder, and make sure it has the following format:

    title: The Wonderful Wizard of Oz
    author: Lyman Frank Baum
    date: 1900/05/17

    Dorothy lived in the midst of the great Kansas prairies, with Uncle Henry, 
    who was a farmer, and Aunt Em, who was the farmer's wife.
  
If one is familiar with webby or aerial, this shouldn't look funny. Basically the top of the file is in YAML format,
and the rest of it is the blog post. They are delimited by an empty line `/\n\n/`, as you can see above. 
None of the information is compulsory, but it's strongly encouraged you specify it.
Note that one can also use `rake` to create an article stub, with `rake new`.

Once he finishes writing his beautiful tale, one can push to the git repo, as usual:

    $ git add articles/wizard-of-oz.txt
    $ git commit -m 'wrote the wizard of oz.'
    $ git push remote master

Where `remote` is the name of your remote git repository. The article is now published.

### server

Toto is built on top of **Rack**, and hence has a **rackup** file: _config.ru_.

#### on your own

You can run toto with any Rack compliant web server, such as **thin**, **mongrel** or **unicorn**.

With thin, you would do something like:

    $ thin start -R config.ru

With unicorn, you can just do:

    $ unicorn

#### on heroku

Toto was designed to work well with heroku:(http://heroku.com), it makes the most out of it's state-of-the-art caching, 
by setting the _Cache-Control_ and _Etag_ HTTP headers.

### configuration

You can configure toto, by modifying the _config.ru_ file. For example, if you want to set the blog author to 'John Galt',
you could add `set :author, 'John Galt'` inside the `Toto::Server.new` block. Here's the options hash with the defaults: 

    :author   => ENV['USER'],                               # blog author 
    :title    => Dir.pwd.split('/').last,                   # site title
    :root     => "index",                                   # page to load on / 
    :date     => lambda {|now| now.strftime("%d/%m/%Y") },  # date format for articles 
    :markdown => :smart,                                    # use markdown + smart-mode 
    :disqus   => false,                                     # disqus id, or false 
    :summary  => 150,                                       # length of article summary 
    :ext      => 'txt'                                      # file extension for articles 

Copyright (c) 2009 cloudhead. See LICENSE for details.
