Encrypted email using ReMail
============================

Remail was written to sidestep the hard-to-solve problem of sending
encrypted mail to multiple people, some of whom may prefer to use GnuPG,
some PGP from Symantec, while others use S/MIME from corporate-issued
CAs that are not in universal CA trust stores.

Remail accepts both S/MIME and PGP-encrypted email sent to a single
address, decrypts it on the back-end, and then re-encrypts it to
individual list subscribers using whichever is their preferred scheme
for exchanging encrypted email.

For more information on this project, please see the `official Remail
git repository`_.

.. _`official Remail git repository`: https://git.kernel.org/pub/scm/linux/kernel/git/tglx/remail.git

Remail at kernel.org
--------------------

Kernel.org uses remail for discussions that need to happen around
coordinated response to embargoed security vulnerabilities. The service
itself runs on a dedicated VM inside a private cloud cluster that has no
direct access from the Internet -- it can only be accessed via the VPN
used by IT operations personnel. Any administrative access to that
internal remail system requires 2-factor authentication. Any off-site
backups performed on that system are PGP-encrypted with a unique
symmetric key before they are uploaded to external storage.

Logging
~~~~~~~

For transparency purposes, conversations exchanged between parties using
encrypted email are logged on the internal remail system in order to
provide a sanitized public discussion archive once embargoes are lifted.

Requesting a remail list
~~~~~~~~~~~~~~~~~~~~~~~~

If you would like to request your own remail list, please contact helpdesk@kernel.org.
