2-factor authentication with gitolite (deprecated)
==================================================

.. warning:: The preferred mechanism for 2-factor authentication is
   via an SSH key stored on a smartcard device. If you have an account
   on kernel.org, you qualify for a free nitrokey (see :doc:`../nitrokey`),
   so we strongly recommend that instead of setting up TOTP/HOTP you
   switch to using the Nitrokey for your ssh access instead.

HOTP/TOTP ip-based push validation
----------------------------------

.. note:: This is entirely opt-in.

=============================== =======================================================================
Command                         Summary
=============================== =======================================================================
``enroll [mode]``               Enroll with 2-factor authentication (mode=totp or yubikey)
``val [token]``                 Validate your current IP address for 24 hours
``val-session [token]``         Validate your current ssh ControlMaster session
``val-for-days [days] [token]`` Validate your current IP address for arbitrary number of days (max=30)
``val-subnet [/cidr] [token]``  Validate a larger subnet for 8 hours
``list-val [all]``              List current validations ("all" includes expired)
``inval [ip-address]``          Invalidate specific IP address (can be "myip", "all" or "all purge")
``isval``                       Check if your current IP is validated
``unenroll [token]``            Unenroll from 2-factor authentication
=============================== =======================================================================

Core concepts
-------------

Once 2-factor authentication is enabled for a git repository, any write
operation from an IP address that hasn't been 2-factor validated will be
rejected with a message like the following::

    remote: User not enrolled with 2-factor authentication.
    remote: FATAL: W VREF/2fa: testing mricon DENIED by VREF/2fa
    remote: 2-factor verification failed
    remote:
    remote: You will need to enroll with 2-factor authentication
    remote: before you can push to this repository.
    remote:
    remote: If you need more help, please see the following link:
    remote:     https://korg.wiki.kernel.org/index.php?title=Userdoc:gitolite_2fa
    remote:
    remote: error: hook declined to update refs/heads/master

To allow the push to succeed, you will need to first validate the IP
address with your 2-factor token, which will allow all pushes from that
IP address to succeed -- until the validation expires. The default
expiration time is 24 hours, but you may set it to be as long as 30
days.

Examples::

    ssh git@gitolite.kernel.org 2fa val XXXXXX
    ssh git@gitolite.kernel.org 2fa val-for-days 30 XXXXXX

Read operations should be completely unaffected.

Supported devices
-----------------

We implement both HOTP (:RFC:`4226`) and TOTP (:RFC:`6238`) parts of the
OATH standard. Since TOTP soft-tokens are easiest to enroll and
provision to such a dispersed crowd as kernel developers -- and since
most kernel developers have access to a smartphone that they carry with
them around anyway -- TOTP is the preferred mechanism.

TOTP apps
~~~~~~~~~

In your smartphone's app directory, you should be able to find one of
the following applications (links given to Google Play store, but should
exist on iOS as well):

* `FreeOTP`_ (preferred, free software)
* `Google Authenticator`_ (proprietary)
* `Authy`_ (proprietary)

.. _`FreeOTP`: https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp
.. _`Google Authenticator`: https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2
.. _`Authy`: https://play.google.com/store/apps/details?id=com.authy.authy

HOTP devices
~~~~~~~~~~~~

The only HOTP devices currently tested and supported are yubikeys. Any
of the currently listed products should support HOTP authentication.

Laptop users may find that using the Nano form factor is more
comfortable.

Provisioning your 2-factor token
--------------------------------

There is a special `2fa` command in gitolite, used to interact with the
2-factor validation component. To provision, you submit a command via
the gitolite interface.

TOTP soft-token apps
~~~~~~~~~~~~~~~~~~~~

To enroll a TOTP soft-token app, such as FreeOTP or Google
Authenticator, run::

    ssh git@gitolite.kernel.org 2fa enroll totp

This command outputs the following::

    totp enrollment mode selected
    New token generated for user mricon

    Please make sure "qrencode" is installed.
    Run the following commands to display your QR code:
        unset HISTFILE
        qrencode -tANSI -m1 -o- "otpauth://totp/mricon@gitolite.kernel.org?secret=ACHQKMJFHIXJDNRY"

    If that does not work or if you do not have access to
    qrencode or a similar QR encoding tool, then you may
    open an INCOGNITO/PRIVATE MODE window in your browser
    and paste the following URL:
    https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth%3A%2F%2Ftotp%2Fmricon%40gitolite.kernel.org%3Fsecret%3DACHQKMJFHIXJDNRY

    Scan the resulting QR code with your TOTP app, such as
    FreeOTP (recommended), Google Authenticator, Authy, or others.
    Please write down/print the following 8-digit scratch tokens.
    If you lose your device or temporarily have no access to it, you
    will be able to use these tokens for one-time bypass.

    Scratch tokens:
    19489805
    36196876
    06341363
    70324458
    39448548

    Now run the following command to verify that all went well
        ssh git@gitolite.kernel.org 2fa val [token]

    If you need more help, please see the following link:
        https://korg.wiki.kernel.org/userdoc:gitolite_2fa

.. note:: Please remember to ``unset HISTFILE`` or your secret will be
   stored in your ~/.bash_history.

Alternatively, if you absolutely have no other way to locally generate a
QR code, **it is important to actually open a new "Incognito/Private
Mode" window to make sure that the URL containing your 2-factor secret
is not saved in your browser history.** Note, that if you do use the
Google link, you'll be giving your 2-factor secret to Google -- whatever
that implies.

Yubikeys
~~~~~~~~

To initialize a yubikey, run the following command instead. Note, that
you will need `ykpersonalize`_ to configure your key::

    ssh git@gitolite.kernel.org 2fa enroll yubikey

The output of the yubikey command is slightly different::

    yubikey enrollment mode selected
    New token generated for user mricon

    Please make sure "ykpersonalize" has been installed.
    Insert your yubikey and, as root, run the following command
    to provision the secret into slot 1 (use -2 for slot 2):
        unset HISTFILE
        ykpersonalize -1 -ooath-hotp -oappend-cr -a7fd554b1e4a711155d20e9f9615b0451152db3bb

    Please write down/print the following 8-digit scratch tokens.
    If you lose your device or temporarily have no access to it, you
    will be able to use these tokens for one-time bypass.

    Scratch tokens:
    88989251
    08286736
    73163062
    90775064
    59235228

    Now run the following command to verify that all went well
        ssh git@gitolite.kernel.org 2fa val [yubkey button press]

    If you need more help, please see the following link:
        https://korg.wiki.kernel.org/userdoc:gitolite_2fa

**It is important to use ``unset HISTFILE`` to make sure the secret
isn't saved in your ~/.bash_history.** Additionally, you may also omit
the -a flag and ``ykpersonalize`` should prompt you for the secret, in
which case paste the string that follows the ``-a`` (but not ``-a``
itself).

.. _`ykpersonalize`: https://developers.yubico.com/yubikey-personalization/

Testing your 2fa token
----------------------

You can test things on the special "testing" repository::

    git clone git@gitolite.kernel.org:testing
    cd testing
    git checkout -b [username]

Edit the README file, commit, and try to push::

    git push origin [username]

You should get the following back::

    Counting objects: 7, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 308 bytes | 0 bytes/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: IP address "x.x.x.x" has not been validated.
    remote: FATAL: W VREF/2fa: testing mricon DENIED by VREF/2fa
    remote: 2-factor verification failed
    remote:
    remote: Please get your 2-factor authentication token and run:
    remote:     ssh git@gitolite.kernel.org 2fa val [token]
    remote:
    remote: If you need more help, please see the following link:
    remote:     https://korg.wiki.kernel.org/index.php/Userdoc:gitolite_2fa
    remote:
    remote: error: hook declined to update refs/heads/mricon
    To git@gitolite.kernel.org:testing
     ! [remote rejected] mricon -> mricon (hook declined)
    error: failed to push some refs to 'git@gitolite.kernel.org:testing'

As instructed, run the following::

    $ ssh git@gitolite.kernel.org 2fa val [token]
    Valid TOTP token within window size used
    Adding IP address x.x.x.x until Wed May 28 20:29:31 2014 UTC
    GeoIP information for x.x.x.x: Saint-laurent, Quebec, CA

If you now try the push again, it will succeed::

    $ git push origin mricon
    Counting objects: 7, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 308 bytes | 0 bytes/s, done.
    Total 3 (delta 1), reused 0 (delta 0)
    remote: Reading /var/lib/gitolite3/repositories/manifest.js.gz
    remote: Updating /testing.git in the manifest
    remote: Writing new /var/lib/gitolite3/repositories/manifest.js.gz
    To git@gitolite.kernel.org:testing
       307ff91..87b27aa  mricon -> mricon

Listing validations and invalidating IPs
----------------------------------------

To list all allowed validations, run::

    $ ssh git@gitolite.kernel.org 2fa list-val
    {
        "172.x.x.x": {
            "added": "2014-05-27 20:27:44+00:00",
            "expires": "2014-05-28 20:27:44+00:00"
        },
        "24.x.x.x": {
            "geoip": "Saint-laurent, Quebec, CA",
            "added": "2014-05-27 20:29:31+00:00",
            "expires": "2014-05-28 20:29:31+00:00"
        }
    }
    Listed non-expired entries only. Run "list-val all" to list all.

Note: this command only works from a whitelisted IP address.

To invalidate an IP, use the "inval" command, e.g.::

    $ ssh git@gitolite.kernel.org 2fa inval 24.x.x.x
    Force-expired 24.x.x.x

Instead of the IP address, you may also use ``myip`` to invalidate the
current IP you're connecting from, or "all" to force-expire all active
IP validations. If you run ``inval all purge``, this will additionally
purge all your current and expired entries -- handy if you would like to
leave no trace of your travel history.

SSH session validation
----------------------

If you are travelling and happen to be behind a single NAT exit point
with a lot of other people, it is preferable to validate only your SSH
session instead of the whole public exit point. This will also help if
the exit point is not static but changes between tcp sessions (as is
sometimes common in very large NAT-ed networks).

Before you can use this feature, you will need to make sure you enabled
**ssh multiplexing** in the client, by adding the following entries to
your gitolite.kernel.org section::

    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlMaster auto
    ControlPersist 30m

You can use longer than 30m if necessary -- the session will be
validated for up to 8 hours. Please see :doc:`../access` for more
ssh setup details.

Using in scripts
----------------

You can check if your current IP is valid from inside a script, by using
the ``isval`` check, e.g. like so:

.. code-block:: bash

   echo -n "Checking if 2fa validation is current: "
   if ! ssh git@gitolite.kernel.org 2fa isval; then
       echo "Error: kernel.org 2fa validation expired"
       exit 1
   fi

Note that there's an inherent race condition here: your validation may
expire between this check and the actual git push.


Switching devices and Unenrolling
---------------------------------

Usually you would need to unenroll only when switching devices. If you
still have access to your current device or to the scratch-tokens, you
can use them to unprovision your current device by using the
``unenroll`` command::

    $ ssh git@gitolite.kernel.org 2fa unenroll [token]
    Valid TOTP token used
    Removing the secrets file.
    Cleaning up state files.
    Expiring all validations.
    Force-expired 172.0.0.14.
    You have been successfully unenrolled.

You can then use the ``enroll`` command again in order to provision a
new device.

If you do NOT have access to your previous 2-factor device, send a
signed email to helpdesk@kernel.org and we'll work to re-provision you a
new token (once a successfully thorough verification procedure is
established and followed).

Requesting 2-factor protection for your repository
--------------------------------------------------

During this opt-in period, send mail to helpdesk@kernel.org to request
that your repository is added to the 2fa list.
