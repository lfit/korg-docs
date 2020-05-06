Getting a kernel.org account
============================
Kernel.org accounts are reserved for Linux kernel maintainers or
high-profile developers. If you do not fall under one of these two
categories, it's unlikely that an account will be issued to you. If you
would like to apply for an account, please e-mail the following
information to helpdesk@kernel.org using the following template::

    Subject: Account request for [your name here]

    preferred username: [username]
    forwarding address: [your@email.address]

    Reasons for requiring a kernel.org account:
    [specify your reasons here]

    [Attachment: export.asc]

The Kernel.org admin team will then review your request and you should
receive a response back within a few days. If you are listed in
`MAINTAINERS`_ and have enough signatures on your PGP key to be in the
web of trust, your account will be issued without delay.

.. important:: If we find an A (authentication) subkey on your PGP key,
   we will assume you will want to use that for your ssh access. If that
   is not the case, please mention it in the request and you'll be
   issued a new ssh private key instead.

.. _`MAINTAINERS`: https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS

Exporting your public key
-------------------------
We no longer rely on keyservers for signature information, so please
attach a copy of your public key to the request. You can generate it
using the following command::

    gpg2 -a --export --export-options export-clean [YOURKEYID] > export.asc

PGP Web of Trust
----------------
.. warning:: With extremely rare exceptions, accounts will not be issued
   unless the there are enough signatures on the PGP key to satisfy the
   web of trust.

We use the PGP web of trust to help ensure that only trusted kernel
developers are able to get an account on kernel.org. Before you send the
email, make sure that your PGP key is signed by *at least two other
people who already have an active kernel.org account*.

PGP signing events at conferences are usually a good place to start, or
you can find kernel developers who live in your area. You can also check
the :doc:`ksmap` for kernel developers in your area. If meeting in
physical space is not an option for you due to travelling or quarantine
restrictions, you may prefer to arrange a video conference call for the
same purpose.

.. important:: Remember, the goal is not to verify someone's
   government-issued credentials, but to build a web of trusted
   contributors. When you are signing someone's key, you are effectively
   stating: "I have worked with this person and I vouch for their
   identity by signing their key with my own."
