How to set up your ssh access
=============================
Setting up your ssh access will depend on whether you're using your PGP Auth
subkey for ssh purposes or if you were issued a private key from kernel.org.

If you received a ssh private key from kernel.org
-------------------------------------------------
Follow this procedure if you received an encrypted tarball containing the SSH
private key to use for accessing your kernel.org account. Place that private
key into your ``~/.ssh`` directory, e.g.::

    cp korg-username ~/.ssh/id_korg

You can change the automatically generated key passphrase using ``ssh-keygen
-p``.

.. important:: You should always keep your ssh key protected by a passphrase.

Add the following entries into your .ssh/config::

    Host gitolite.kernel.org
      User git
      IdentityFile ~/.ssh/id_korg
      IdentitiesOnly yes
      ClearAllForwardings yes
      # We prefer ed25519 keys, but will fall back to others if your
      # openssh client does not support that
      HostKeyAlgorithms ssh-ed25519,ecdsa-sha2-nistp256,ssh-rsa
      # Below are very useful for speeding up repeat access
      # and for 2-factor validating your sessions
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 30m
      # Helps behind some NAT-ing routers
      ServerAliveInterval 60

If we used your PGP Authentication subkey
-----------------------------------------
If we found an Authentication (**[A]**) subkey on your PGP key, then we have
set up your access to use that key, instead of creating new ssh private keys.
This is what you need to do to configure your ssh client to use that subkey:

First, add the following to your ``~/.gnupg/gpg-agent.conf``::

    enable-ssh-support

Then, add this to your ``.bashrc``::

    export SSH_AUTH_SOCK=$(gpgconf --list-dirs agent-ssh-socket)

You will need to kill the existing gpg-agent process and start a new login
session for the changes to take effect::

    $ killall gpg-agent
    $ bash
    $ ssh-add -L

The very first entry in the output should be the ssh public key derived from
your PGP Auth subkey -- it should have "``cardno:XXXXXXXX``" at the end in the
comment section.

Now add this to your .ssh/config::

    Host gitolite.kernel.org
      User git
      ClearAllForwardings yes
      # We prefer ed25519 keys, but will fall back to others if your
      # openssh client does not support that
      HostKeyAlgorithms ssh-ed25519,ecdsa-sha2-nistp256,ssh-rsa
      # Below are very useful for speeding up repeat access
      # and for 2-factor validating your sessions
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlMaster auto
      ControlPersist 30m
      # Helps behind some NAT-ing routers
      ServerAliveInterval 60

SSH host fingerprints
---------------------
Your kernel.org account grants you access to gitolite.kernel.org, which
you will use both  for accessing your git trees (see
:doc:`gitolite/index`) and for uploading tarball releases (see
:doc:`kup`).

======= =======================================================
Key     MD5 Fingerprint
======= =======================================================
RSA     ``MD5:b1:33:44:9d:3f:77:59:14:f8:05:d7:33:5d:b1:40:7b``
ECDSA   ``MD5:7c:a6:a2:e0:96:5f:e2:9a:9b:53:b6:41:29:66:f8:47``
ED25519 ``MD5:30:f1:e6:8f:ff:76:45:e7:5b:45:b0:bd:bd:ca:14:9c``
======= =======================================================

======= =======================================================
Key     SHA256 Fingerprint
======= =======================================================
RSA     ``SHA256:S1b2ARCfjjhsPJeqbCwkG+2ukBPCApogEfRTkVqEj4g``
ECDSA   ``SHA256:n5cYLTSXgZ97jR9DfOcFxHeHAt3BBqU89TpTQspqFxo``
ED25519 ``SHA256:KTfZsrwphTMpYOYr0Acfdk25gtg6zui3Oh8QOawAm5M``
======= =======================================================

Here they are PGP-signed::

    -----BEGIN PGP SIGNED MESSAGE-----
    Hash: SHA256

    # ssh-keygen -E sha256 -lf <(ssh-keyscan gitolite.kernel.org)
    2048 SHA256:S1b2ARCfjjhsPJeqbCwkG+2ukBPCApogEfRTkVqEj4g gitolite.kernel.org (RSA)
    256  SHA256:n5cYLTSXgZ97jR9DfOcFxHeHAt3BBqU89TpTQspqFxo gitolite.kernel.org (ECDSA)
    256  SHA256:KTfZsrwphTMpYOYr0Acfdk25gtg6zui3Oh8QOawAm5M gitolite.kernel.org (ED25519)

    # ssh-keygen -E md5 -lf <(ssh-keyscan gitolite.kernel.org)
    2048 MD5:b1:33:44:9d:3f:77:59:14:f8:05:d7:33:5d:b1:40:7b gitolite.kernel.org (RSA)
    256  MD5:7c:a6:a2:e0:96:5f:e2:9a:9b:53:b6:41:29:66:f8:47 gitolite.kernel.org (ECDSA)
    256  MD5:30:f1:e6:8f:ff:76:45:e7:5b:45:b0:bd:bd:ca:14:9c gitolite.kernel.org (ED25519)
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v2

    iQIcBAEBCAAGBQJZEg1UAAoJEDS6uAr58ke44k0QAKQ2mdfN9aebDBmt4BpBcIHo
    DFZ8CN9/NTzJz7ZuYuwkgeWj1Ah1wWEb36q7UojA5Iq7BxLjP1jpZZlRSaQnyTXV
    87/7DUcEshS3wasRatCJ+GhBtQ2WJAblVHs2BVpPffJT+KhwSM0vzhnME41ppWtJ
    poZ/8UO1qlPZKjUutFeS7ogDC5te5BTEDAQuQLMUMgi1rzRYJvdIeIymgr0Hk4IA
    8Ss+7sH0vj5p0hd2tNS+FpGXQORnKb4VYWupsG7tJfVRloEBKFly8oOPGGR/nHxg
    vWJQl2Nc+05Vf5ey0bKWBlWyhFuuPlxFMPdCPQCKQMrAhTTAbtYAk71kmaxJQH0P
    QeE8u/qLS4GYaSktPhjh+vYFNlwqPQ3WDwye3mXZN35eUTXgQX0beJBEBWGLdQsH
    UpBUnTB5U0mzA4uNCOh1yfqaGjwdFru/c2ivA6e59SRoijjJOSL2+PLw/pHXbCSQ
    AIGo7ysfF4V2EDZ6A234NYaI1PTGPt+hLRBxkzOONUjxiIoDAuRcrTHFz9oAtnvA
    Xy1CAxTLXpgeCjJEN2s0EQgrEFeB/GDOfWBq/Z3itGBo1UD5HvOuYAay/tLgVzur
    0/TTefeni1uFvl9kU3zsNiqm9YI2IaKPa4SMTmjEZSPlaTuuxApw5G1EBmCXexHS
    YjQNG2ORVjiVyvdddWT9
    =Njcj
    -----END PGP SIGNATURE-----
