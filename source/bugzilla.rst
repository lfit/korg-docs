Using bugzilla.kernel.org
=========================
Getting bug reports for components
----------------------------------
The kernel.org bugzilla uses a mechanism that allows anyone to
subscribe/unsubscribe from bug reports for specific components. This is
done using virtual default assignees and "watched user" functionality of
bugzilla.

Real assignees vs. virtual assignees
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
People maintaining various kernel modules change routinely and in order
to make it easier for people to start/stop receiving bugzilla notices
for various components, bugzilla.kernel.org often uses "virtual default
assignees" like "drivers_foo@kernel-bugs.osdl.org" or
"drivers_bar@kernel-bugs.kernel.org", which are not real deliverable
addresses (on purpose). Maintainers then subscribe to bug reports that
are assigned to these virtual addresses and are thus able to receive the
bug reports without involving bugzilla administrators.

To start getting bug reports for a component
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Create an account on bugzilla.kernel.org if you do not yet have one
- Find out the default assignee for the component you are interested in:

    - Click on **Browse** in the top bar
    - Click on the category you are interested in
    - Copy the email address of the default assignee for that component

- Once you have the default assignee's email address, click on
  **Preferences** in the top bar
- Select the **Email Notifications** tab
- Scroll to the bottom, where you will find the **User Watching** section
- Add the email address of the default assignee to the field. If you are
  watching multiple users, the entries are comma-separated.
- Submit changes

To stop receiving bug reports for a component
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Log in to bugzilla.kernel.org
- Click on **Preferences** in the top bar
- Select the **Email Notifications** tab
- Scroll to the bottom, where you will find the **User Watching** section
- Remove the email address you no longer want to shadow
- Submit changes

Sending bugmail to mailing lists
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Send a request to helpdesk@kernel.org with the address of the list and
the name of the bugzilla component for which it should receive bugmail.

Why can't I edit bug component/description/etc?
-----------------------------------------------
You need to be added to the special "editbugs" group. Just send a
request to helpdesk@kernel.org and you will be added to the group,
allowing you to edit any aspect of a bug.
