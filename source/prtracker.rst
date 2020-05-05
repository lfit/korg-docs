Pull Request tracker bot
========================
The pull request tracker bot sends automatic notifications when a pull
request submitted to a mailing list has been merged to one of the Linux
kernel repositories.

.. note:: At this time, it is only used to track mainline pull requests
   sent to LKML. We intend to make this a generic service usable by
   other developers after the initial testing period.

I don't want these!
-------------------
If you don't want to receive these notices, you have three ways of opting out:

  - Block all mail from pr-tracker-bot@kernel.org
  - CC your pull requests to pr-tracker-bot+nothanks@kernel.org
  - Mention "pr-tracker-no-ack" anywhere in the request body

The first one is the most effective and can be done in one click of a
button in most email clients.

Source code
-----------
You can find the source for the bot in the helpers repository:

* https://git.kernel.org/pub/scm/linux/kernel/git/mricon/korg-helpers.git/tree/pr-tracker-bot.py
