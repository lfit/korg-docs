people.kernel.org
=================
We partnered with write.as to offer an Activity-Pub federated blogging
instance for kernel developers.

Requesting your blog
--------------------
This service is offered to anyone with a kernel.org account, or anyone
with an entry in the MAINTAINERS file. If you do not fall under one of
these two categories, you can get someone else to "sponsor" your
request.

For further details, please see https://people.kernel.org/about.

Quickstart
----------
The configuration interface is pretty minimal and self-explanatory. Here
are a few pointers to get you started:

- All blogs start out as "unlisted" meaning that they won't show up on
  people.kernel.org/read until you make them public -- but they are
  still accessible as people.kernel.org/username. To add them to the
  global feed, go to https://people.kernel.org/me/c/, click "Customize"
  and change "Publicity" to "public".
- Your posts are published into "Draft" by default, which lets you
  preview them and share with others under a non-guessable link. To make
  them public in your blog, click on "Drafts" in the title bar (or "View
  drafts" in the menu), and then on "move into yourblogname" next to the
  post that you want to make public.
- If you want to follow people.kernel.org in your RSS reader, the feed
  it at https://people.kernel.org/read/feed/ (the last / is important).
  For just your own blog's RSS feed, s/read/yourblogname/.

Code snippets
~~~~~~~~~~~~~
If you're posting a code snippet, add the language identifier next to
the opening stanza. E.g.::

    ```c
    #include <stdio.h>

    int main()
    {
       printf("Hello, World!");
    }
    ```
