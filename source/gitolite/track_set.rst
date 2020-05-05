Setting repo remotes with the "track" command
=============================================
This is useful if you want to avoid sending a lot of objects when
performing ``git push`` and want to copy those objects from another
local repository first.

========================================================== ==========================================
Subcommand                                                 Summary
========================================================== ==========================================
``set your-repo-path orig-repo-path remote-name [--tags]`` Runs ``git remote add [--tags]``
``fetch your-repo-path remote-name``                       Perform ``git fetch --prune``
``rm your-repo-path remote-name``                          Perform ``git remote remove``
``list** your-repo-path``                                  List remotes configured in your repository
========================================================== ==========================================

.. note:: It is almost never useful to track torvalds/linux.git, as our
   use of git alternates will already make sure that you don't have to
   push objects already present in torvalds/linux.git. This command is
   useful for objects that have not yet found their way into Linus's
   repo (e.g. those from linux-next).

Examples
--------
All examples are for linux-next, since that's likely to be what most
people want.

Setting
~~~~~~~
Set up linux-next as a remote for your own repository::

    ssh git@gitolite.kernel.org track set \
        pub/scm/linux/kernel/git/mricon/linux \    # <-- Your repository path
        pub/scm/linux/kernel/git/next/linux-next \ # <-- The repository you want
        linux-next [--tags]                        # <-- remote name (pass --tags if you want tags)

This will do the following::

    cd $REPOS/pub/scm/linux/kernel/git/mricon/linux.git
    git remote add --no-tags linux-next $REPOS/pub/scm/linux/kernel/git/next/linux-next.git
    git fetch linux-next --prune

If you passed ``--tags`` at the end of the command, then ``--no-tags``
will be omitted and you'll get all the tags from the remote repository
as well.

Fetching
~~~~~~~~
Right before you do ``git push``, run the following command to fetch the
latest objects from linux-next and thus avoid pushing them over your ssh
connection during your own ``git push``::

    ssh git@gitolite.kernel.org track fetch \
        pub/scm/linux/kernel/git/mricon/linux \ # <-- Your repository path
        linux-next                              # <-- Remote name you want to fetch

This will do the following::

    cd $REPOS/pub/scm/linux/kernel/git/mricon/linux.git
    git fetch linux-next --prune

Listing
~~~~~~~
Forgot which remotes you already set up?

::

    ssh git@gitolite.kernel.org track list \
        pub/scm/linux/kernel/git/mricon/linux # <-- Your repository path

Removing
~~~~~~~~
Done with a remote? You can just leave it there, or remove it entirely::

    ssh git@gitolite.kernel.org track rm \
        pub/scm/linux/kernel/git/mricon/linux \ # <-- Your repository path
        linux-next                              # <-- Remote name you want to remove

This will do the following::

    cd $REPOS/pub/scm/linux/kernel/git/mricon/linux.git
    git remote remove linux-next
