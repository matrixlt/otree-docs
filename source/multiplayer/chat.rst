.. _chat:

Chat
====

You can add a chat room to a page so that participants can communicate with each other.

Basic usage
~~~~~~~~~~~

In your template HTML, put:

.. code-block:: html

    {{ chat }}

This will make a chat room among players in the same Group,
where each player's nickname is displayed as
"Player 1", "Player 2", etc. (based on the player's ``id_in_group``).

Customizing the nickname and chat room members
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can specify a ``channel`` and/or ``nickname`` like this:

.. code-block:: html

    {{ chat nickname="abc" channel="123" }}

Nickname
''''''''

``nickname`` is the nickname that will be displayed for that user in the chat.
A typical usage would be ``{{ chat nickname=player.role }}``.

Channel
'''''''

``channel`` is the chat room's name, meaning that if 2 players
have the same ``channel``, they can chat with each other.
``channel`` is not displayed in the user interface; it's just used internally.
Its default value is ``group.id``, meaning all players in the group can chat together.
You can use ``channel`` to instead scope the chat to the current page
or sub-division of a group, etc. (see examples below).
Regardless of the value of ``channel``,
the chat will at least be scoped to players in the same session and the same app.

Example: chat by role
`````````````````````

Here's an example where instead of communication within a group,
we have communication between groups based on role,
e.g. all buyers can talk with each other,
and all sellers can talk with each other.


.. code-block:: python

    def chat_nickname(player):
        group = player.group

        return 'Group {} player {}'.format(group.id_in_subsession, player.id_in_group)

In the page:

.. code-block:: python

    class MyPage(Page):

        @staticmethod
        def vars_for_template(player):
            return dict(
                nickname=chat_nickname(player)
            )

Then in the template:

.. code-block:: html

    {{ chat nickname=nickname channel=player.id_in_group }}

Example: chat across rounds
```````````````````````````

If you need players to chat with players who are currently in a different round
of the game, you can do:

.. code-block:: html

    {{ chat channel=group.id_in_subsession }}

Example: chat between all groups in all rounds
``````````````````````````````````````````````

If you want everyone in the session to freely chat with each other, just do:

.. code-block:: html

    {{ chat channel=1 }}

(The number 1 is not significant; all that matters is that it's the same for everyone.)

Advanced customization
~~~~~~~~~~~~~~~~~~~~~~

If you look at the page source code in your browser's inspector,
you will see a bunch of classes starting with ``otree-chat__``.

You can use CSS or JS to change the appearance or behavior of these elements
(or hide them entirely).

You can also customize the appearance by putting it inside a ``<div>``
and styling that parent ``<div>``. For example, to set the width:

.. code-block:: html

    <div style="width: 400px">
        {{ chat }}
    </div>

Multiple chats on a page
~~~~~~~~~~~~~~~~~~~~~~~~

You can have multiple ``{{ chat }}`` boxes on each page,
so that a player can be in multiple channels simultaneously.

Exporting CSV of chat logs
~~~~~~~~~~~~~~~~~~~~~~~~~~

The chat logs download link will appear on oTree's regular data export page.

Chat between participants and experimenter
------------------------------------------

See :ref:`experimenter-chat`.