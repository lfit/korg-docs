Using kernel.org uploader (kup)
===============================

Kernel.org uploader (kup) is a Perl program that was written for the
sole purpose of making sure that any software uploaded to kernel.org is
cryptographically verified against a list of pre-approved maintainers.
If you are a kernel developer and you'd like to be able to upload
tarballs, patches or any other data to https://kernel.org/pub/, this
document is for you.

Before you begin
----------------

.. warning:: Do not upload non-Open Source software without special
   prior permission. This includes binaries of software intended to be
   Open Source for which source code isn't actually available yet. IN
   PARTICULAR, do not upload non-Open Source cryptographic software, or
   cryptographic binaries from source not available on the Internet,
   under any circumstances. This is absolutely essential.

If you have not yet set up your ssh access, first see :doc:`access`.

Obtaining kup
-------------

Kup is packaged for most distributions and is probably installable using
your distribution's package management tools. For example, on Fedora it
is installable using::

    yum install kup

.. note:: Make sure the version of kup is at least 0.3.6, as earlier
   versions will NOT work.

You can also check out the git tree to obtain the latest version of
kup::

    git clone git://git.kernel.org/pub/scm/utils/kup/kup.git

Configuring kup
---------------

Set your ``$HOME/.kuprc`` as follows (**requires kup >= 0.3.6**)::

    host = git@gitolite.kernel.org
    subcmd = kup-server

That should be all you need to do. You may test things by running::

    kup info

You should see the following or similar output::

    kup-server 0.3.6 (gitolite integrated)

    ls                       /*
    put/rm/mv/mkdir          /testing/*
    put/rm/mv/mkdir          /pub/linux/kernel/people/yourname/*

If you get an error in return, check to make sure you have correctly set
up your ssh access (see :doc:`access`). Additionally, if you see a long
list of gitolite repositories instead, make sure your version of kup is
0.3.6 or above, and that you have correctly set up your ``~/.kuprc`` to
specify ``subcmd=kup-server``.

If it is still not working for you, please email helpdesk@kernel.org for
further assistance.

Uploading with kup
------------------

Please carefully read the manpages provided by kup to familiarize
yourself with the procedure::

    man kup

You can test kup by uploading into the special ``/testing`` directory::

    kup put foo.tar foo.tar.asc /testing/foo.tar.gz

If you get "invalid signature", the most likely cause is that your
signature is against the compressed version of the archive. KUP expects
the signature to be against .tar, not .tar.?z. It uncompresses the file,
then runs the verification check, then recompresses in multiple formats.

Please remove anything you put there after you are done::

    kup rm /testing/foo.tar.gz

Anything uploaded to ''/pub'' will be rsynced to kernel.org frontends
and will be available on http://kernel.org/pub after a few minutes.

Permissions
-----------

If you find that some directories do not have the correct permissions,
or if you need to set up groups in order to allow multiple developers
write access, please send a request to helpdesk@kernel.org.
