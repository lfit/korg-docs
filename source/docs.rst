RTD (Read The Docs)
===================
If you would like kernel.org to host documentation for your project,
component, or subtree, then the best way to do that is to host it on
readthedocs.org with a custom foo.docs.kernel.org subdomain.

Hosting on git.kernel.org vs. GitHub/Gitlab
-------------------------------------------
Since you probably want to make it easy for someone to contribute
drive-by edits, the best option is to host the source of your
documentation on GitHub/Gitlab, which would allow people to easily fork
and submit pull requests for any doc edits. There is a lot of
pre-existing integration between GitHub/Gitlab and RTD, so this is the
path of least resistance.

You can also request to set up a tree on git.kernel.org under
``pub/scm/docs`` and either push a copy of your commits there, or use it
as the master source of your documentation, in which case we will need
to manually integrate it with RTD.

Requesting a hook integration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you do decide to host the master source of your docs on
git.kernel.org, then you can request hook integration. Send an email to
helpdesk@kernel.org providing all the necessary parameters for the
webhook curl call, as described in RTD documentation:

* https://docs.readthedocs.io/en/stable/webhooks.html#parameters

Requesting a docs.kernel.org subdomain
--------------------------------------
Once you have your documentation ready, please send a request to
helpdesk@kernel.org, specifying:

- the current shared-domain URL of your RTD project
- the docs.kernel.org subdomain you would like

Once we review the content on the shared-domain site, we will set up the
CNAME record for your subdomain and you will then be able to configure
it on readthedocs.org.
