=========================================
Electric Hands Software Mac code snippets
=========================================

:author: Grzegorz Adam Hankiewicz <elhaso@gradha.imap.cc>

General
=======

It is inevitable to repeat pieces of code when you write multiple applications.
While general libs are good, there will always be snippets of code which don't
have place in a full fledged library and just end up being copied and pasted
everywhere. Then you improve some code and begin hunting all the places were
you need to replace it. Inevitably you will end up with mixed versions
everywhere and no end to the search and replace task.

This problem is even bigger in the Objective-C world. Due to Objective-C
categories (http://en.wikipedia.org/wiki/Objective-C#Categories) being similar
in spirit to header macros, at some point you will definitely say to yourself
"*Damn, I hate how NSArray doesn't have a method to get objects without
throwing out of bounds exceptions*" and write a short helper which now you are
copying to every one of your projects.

I've been there, and found that most of the time you get attached to *your*
helpers/categories/macros but it's a pain to either set up or copy everything
everywhere. Since most of my code lives now in a github repository, I've
decided to put all my little snippets of wisdom/stupidity in a single place
so I can easily update them. Using git submodules I can bring my code up to
date everywhere I use it.

Feel free to grab stuff from here into your project, or improve on it. Even if
nothing from here is useful to you, maybe the idea prompts you to make your own
repository and in the process save yourself some future pain. That's still a
win!


Installation
------------

The files
*********

For the purpose of tracking external source code, I recommend you to create an
``external`` subdirectory at the root of your own project. Then, you can either
download and copy this repository there under the name ``ELHASO-mac-snippets``,
or you can checkout a submodule. For the git submodule you would do::

    cd Your-project
    mkdir external
    git submodule init
    git submodule add \
        git://github.com/gradha/ELHASO-mac-snippets.git \
        external/ELHASO-mac-snippets

These commands would create an ``external`` directory and populate it with the
source code. For the github url of the repo you could use my repo, or you could
fork it and use your own. This is recommended: if I removed my repo, yours
would still live, and it is also more obvious to watch/track different changes
among repositories.

If you are *not* using git, I recommend you to modify the ``README.rst`` file
and make it contain a timestamp, date, or github checkout sha-1 so that in the
future you know if new versions have been released by comparing this mark.


Your Xcode project settings
***************************

Now that the files somehow live inside your project, you have to tell Xcode to
find them. For testing reasons, add the following line to one of your **.m**
files::

    #include "ELHASO.h"

After this change your project should not build due to errors. Open the
project's information, and in the **Build** tab find the **Header Search
Paths** setting. Add to this value (if you already have other settings) the
following line::

    external/ELHASO-mac-snippets/src

After this change your project should build again. However, this will only work
for the header files, since the source code is still not being linked by Xcode,
and therefore using any of the categories will fail during the link phase.
Create in your project's file tree a new **external** group. Then, right
clicking on the group select "**Add/Existing files...**". Navigate and add the
``src`` subdirectory. You don't want to include anything else like this README
or the example directory.

If you are using ARC in your project, now that you've added source files to
your target you need to specify the ``-fno-objc-arc`` parameter for them.
Navigate to your target's build phases and filter the "**Compile Sources**"
section by the prefix ``EH`` and later by the word ``ELHASO`` to find the files
you need to add this parameter for.

Now using a category in your project should work.  However, the project outline
looks weird with this **external/src** branch.  Rename the **src** part to
**ELHASO-mac-snippets** (or something shorter) so you can recognise it more
easily. This will not affect your build, since Xcode groups don't necessarily
match directories and/or files.  Now you can include any other file like
``NSArray+ELHASO.h`` in your source code and use their code.

One last step would be to define custom macros for your project, since there
are some preprocessor tricks used by this code. In your project's build
settings look for the gcc option "**Preprocessor Macros**". Before changing
anything, verify that instead of "**All configurations**" selected in the
listbox you are on either **Debug** or **Release**. Define the following
macros:

* For Debug settings: **DEBUG**
* For Release settings: **NDEBUG**, **NS_BLOCK_ASSERTIONS** and **RELEASE**

If you don't define these preprocessor macros for your projects handy macros
like DLOG or RASSERT may not do what you expect them to do. Feel free to leave
other of your macros or add your own.


Documentation
-------------

Good luck!


License
-------

This code is provided mostly under the MIT license, for details see the
`LICENSE.rst file <LICENSE.rst>`_.
