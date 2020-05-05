How to request a new project on patchwork
=========================================
Patchwork_ is a tool to help track patches sent to a mailing list, to
make sure they are not lost in the traffic or otherwise overlooked.

.. _Patchwork: https://patchwork.kernel.org/

To get a new project added to patchwork, please send an email request to
helpdesk@kernel.org with the following info:

- The website with the details about the mailing list (e.g. mailman listinfo page)
- The patchwork.kernel.org username of the person (or persons) who will
  be the project admin in patchwork. If you don't have a username yet,
  please create one on patchwork.kernel.org.
- Patchwork-bot integration details, if desired (see below)

Adding patchwork-bot integration
--------------------------------
If you have your git tree on git.kernel.org, you can send a request to
helpdesk@kernel.org to request patchwork-bot integration. Please
provide all of the following info as part of your request:

- Patchwork project URL: (e.g. https://patchwork.kernel.org/project/linux-kselftest/)
- Git URL: (e.g. https://git.kernel.org/pub/scm/linux/kernel/git/shuah/linux-kselftest.git/)
- Refname:state map: (e.g. refs/heads/next:Accepted, refs/heads/master:Mainlined)
- Summary to: (e.g. example@kernel.org, example@gmail.com)
- Notify submitters?: (yes/no)
- (If yes on previous) Cc the list on notifications? (yes/no)
- Auto-supersede series? (yes, within NN days/no)
- Auto-archive old patches? (yes, when older than NN days/no)

Auto-superseding series and archiving old patches
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The bot can do some basic housekeeping chores, such as automatically
marking patches belonging to older versions of the series as
"superseded." E.g. if a contributor sends ``[PATCH NN/30] Do foo to
bar``, and then after that a new revision ``[PATCH v2 NN/30] Do foo to
bar``, the bot can automatically mark the 30 patches belonging to the
``v1`` of the series as "superseded". In order to work, the following
conditions must be met:

- It must be submitted by the same person
- It must have the exact same series name (first patch or cover letter
  subject wording)
- It must be within the cutoff period of days specified. In other word,
  if the cutoff is 90 days and the new series comes in 4 months later,
  there will be no match

Similarly, the bot can archive patches older than a certain period of
time if they are still in the "New" state.

Notifying submitters
~~~~~~~~~~~~~~~~~~~~
If you choose to notify submitters, it would send them a summary email
per each series that was marked as accepted, for example::

    Subject: Re: [PATCH v3,00/03] Apply foo to bar
    From: patchwork-bot+project-name@kernel.org
    To: Submitter Name <submitter@kernel.org>

    Hello:

    This series was applied to shuah/linux-kselftest (refs/heads/fixes).

    On Fri, 07 Dec 2018 11:09:48 +0100 you wrote:
    > The foo is not applied to bar, but it should be.
    >
    > Signed-off-by: Awesome Contributor <awesome@example.com>

    Here is a summary with links:
      - [1/3] Apply foo to bar
        https://git.kernel.org/[...]
      - [2/3] Apply foo to bar with more conviction
        https://git.kernel.org/[...]
      - [3/3] Apply foo to bar for real this time
        https://git.kernel.org/[...]

    You are awesome, thank you!

    --
    Deet-doot-dot, I am a bot.
    https://korg.wiki.kernel.org/userdoc/pwbot

This way, if you are applying a series of 50 patches from the same
person, the submitter will only receive a single notification email.
