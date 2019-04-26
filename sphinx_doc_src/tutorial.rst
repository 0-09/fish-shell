.. highlight:: fish-docs-samples

Tutorial
========

Why fish?
---------

``fish`` is a fully-equipped command line shell (like bash or zsh) that is smart and user-friendly. ``fish`` supports powerful features like syntax highlighting, autosuggestions, and tab completions that just work, with nothing to learn or configure.

If you want to make your command line more productive, more useful, and more fun, without learning a bunch of arcane syntax and configuration options, then ``fish`` might be just what you're looking for!


Getting started
---------------

Once installed, just type in ``fish`` into your current shell to try it out!

You will be greeted by the standard fish prompt,
which means you are all set up and can start using fish::

    > fish
    <outp>Welcome to fish, the friendly interactive shell</outp>
    <outp>Type <span class="cwd">help</span> for instructions on how to use fish</outp>
    you@hostname ~>____


This prompt that you see above is the ``fish`` default prompt: it shows your username, hostname, and working directory.
- to change this prompt see `how to change your prompt <prompt>`_
- to switch to fish permanently see `switch your default shell to fish <#switching-to-fish>`_.

From now on, we'll pretend your prompt is just a '``>``' to save space.


Learning fish
-------------

This tutorial assumes a basic understanding of command line shells and Unix commands, and that you have a working copy of ``fish``.

If you have a strong understanding of other shells, and want to know what ``fish`` does differently, search for the magic phrase <em>unlike other shells</em>, which is used to call out important differences.


Running Commands
----------------

``fish`` runs commands like other shells: you type a command, followed by its arguments. Spaces are separators::

    >_ echo hello world
    <outp>hello world</outp>


You can include a literal space in an argument with a backslash, or by using single or double quotes::

    >_ mkdir My\ Files
    >_ cp ~/Some\ File 'My Files'
    >_ ls "My Files"
    <outp>Some File</outp>


Commands can be chained with semicolons.


Getting Help
------------

``fish`` has excellent help and man pages. Run ``help`` to open help in a web browser, and ``man`` to open it in a man page. You can also ask for help with a specific command, for example, ``help set`` to open in a web browser, or ``man set`` to see it in the terminal.



::

    >_ man set
    <outp>set - handle shell variables</outp>
    <outp>  Synopsis...</outp>



Syntax Highlighting
-------------------

You'll quickly notice that ``fish`` performs syntax highlighting as you type. Invalid commands are colored red by default::

    >_ <eror>/bin/mkd</eror>


A command may be invalid because it does not exist, or refers to a file that you cannot execute. When the command becomes valid, it is shown in a different color::

    >_ /bin/mkdir


``fish`` will underline valid file paths as you type them::

    >_ cat <u>~/somefi</u>___


This tells you that there exists a file that starts with '``somefi``', which is useful feedback as you type.

These colors, and many more, can be changed by running ``fish_config``, or by modifying variables directly.


Wildcards
---------

``fish`` supports the familiar wildcard ``*``. To list all JPEG files::

    >_ ls *.jpg
    <outp>lena.jpg</outp>
    <outp>meena.jpg</outp>
    <outp>santa maria.jpg</outp>


You can include multiple wildcards::

    >_ ls l*.p*
    <outp>lena.png</outp>
    <outp>lesson.pdf</outp>


Especially powerful is the recursive wildcard ** which searches directories recursively::

    >_ ls /var/**.log
    <outp>/var/log/system.log</outp>
    <outp>/var/run/sntp.log</outp>


If that directory traversal is taking a long time, you can :kbd:`Control+C` out of it.


Pipes and Redirections
----------------------

You can pipe between commands with the usual vertical bar::

    >_ echo hello world | wc
    <outp>       1       2      12</outp>


stdin and stdout can be redirected via the familiar `<` and `<`. stderr is redirected with a `2>`.



::

    >_ grep fish < /etc/shells > ~/output.txt 2> ~/errors.txt



Autosuggestions
---------------

``fish`` suggests commands as you type, and shows the suggestion to the right of the cursor, in gray. For example::

    >_ <eror>/bin/h</eror><s>___ostname</s>


It knows about paths and options::

    >_ grep --i<s>___gnore-case</s>


And history too. Type a command once, and you can re-summon it by just typing a few letters::

    >_ <eror>r<</eror><s>___sync -avze ssh . myname@somelonghost.com:/some/long/path/doo/dee/doo/dee/doo</s>


To accept the autosuggestion, hit :kbd:`→` (right arrow) or :kbd:`Control+F`. To accept a single word of the autosuggestion, :kbd:`Alt+→` (right arrow). If the autosuggestion is not what you want, just ignore it.

Tab Completions
---------------

``fish`` comes with a rich set of tab completions, that work "out of the box."

Press :kbd:`Tab`, and ``fish`` will attempt to complete the command, argument, or path::

    >_ <eror>/pri</eror> :kbd:`Tab` => /private/


If there's more than one possibility, it will list them::

    >_ <eror>~/stuff/s</eror> :kbd:`Tab`
    <outp><mtch>~/stuff/s</outp>cript.sh  <i>(Executable, 4.8kB)</i>  \mtch{~/stuff/s</mtch>ources/  <i>(Directory)</i>}


Hit tab again to cycle through the possibilities.

``fish`` can also complete many commands, like git branches::

    >_ git merge pr :kbd:`Tab` => git merge prompt_designer
    >_ git checkout b :kbd:`Tab`
    <outp><mtch>b</outp>uiltin_list_io_merge <i>(Branch)</i> \mtch{b</mtch>uiltin_set_color <i>(Branch)</i> <mtch>b</mtch>usted_events <i>(Tag)</i>}


Try hitting tab and see what ``fish`` can do!

Variables
---------

Like other shells, a dollar sign performs variable substitution::

    >_ echo My home directory is $HOME
    <outp>My home directory is /home/tutorial</outp>


Variable substitution also occurs in double quotes, but not single quotes::

    >_ echo "My current directory is $PWD"
    <outp>My current directory is /home/tutorial</outp>
    >_ echo 'My current directory is $PWD'
    <outp>My current directory is $PWD</outp>


Unlike other shells, ``fish`` has no dedicated syntax for setting variables. Instead it has an ordinary command: ``set``, which takes a variable name, and then its value.



::

    >_ set name 'Mister Noodle'
    >_ echo $name
    <outp>Mister Noodle</outp>


(Notice the quotes: without them, ``Mister`` and ``Noodle`` would have been separate arguments, and ``$name`` would have been made into a list of two elements.)

Unlike other shells, variables are not further split after substitution::

    >_ mkdir $name
    >_ ls
    <outp>Mister Noodle</outp>


In bash, this would have created two directories "Mister" and "Noodle". In ``fish``, it created only one: the variable had the value "Mister Noodle", so that is the argument that was passed to ``mkdir``, spaces and all. Other shells use the term "arrays", rather than lists.


Exit Status
-----------

Unlike other shells, ``fish`` stores the exit status of the last command in ``$status`` instead of ``$?``.



::

    >_ false
    >_ echo $status
    <outp>1</outp>


Zero is considered success, and non-zero is failure. There is also a ``$pipestatus`` array variable for the exit statues of processes in a pipe.


Exports (Shell Variables)
-------------------------

Unlike other shells, ``fish`` does not have an export command. Instead, a variable is exported via an option to ``set``, either ``--export`` or just ``-x``.



::

    >_ set -x MyVariable SomeValue
    >_ env | grep MyVariable
    <outp><m>MyVariable</outp>=SomeValue</m>


You can erase a variable with ``-e`` or ``--erase``



::

    >_ set -e MyVariable
    >_ env | grep MyVariable
    <outp>(no output)</outp>



Lists
-----

The ``set`` command above used quotes to ensure that ``Mister Noodle`` was one argument. If it had been two arguments, then ``name`` would have been a list of length 2.  In fact, all variables in ``fish`` are really lists, that can contain any number of values, or none at all.

Some variables, like ``$PWD``, only have one value. By convention, we talk about that variable's value, but we really mean its first (and only) value.

Other variables, like ``$PATH``, really do have multiple values. During variable expansion, the variable expands to become multiple arguments::

    >_ echo $PATH
    <outp>/usr/bin /bin /usr/sbin /sbin /usr/local/bin</outp>


Note that there are three environment variables that are automatically split on colons to become lists when fish starts running: ``PATH``, ``CDPATH``, ``MANPATH``. Conversely, they are joined on colons when exported to subcommands. All other environment variables (e.g., ``LD_LIBRARY_PATH``) which have similar semantics are treated as simple strings.

Lists cannot contain other lists: there is no recursion.  A variable is a list of strings, full stop.

Get the length of a list with ``count``::

    >_ count $PATH
    <outp>5</outp>


You can append (or prepend) to a list by setting the list to itself, with some additional arguments. Here we append /usr/local/bin to $PATH::

    >_ set PATH $PATH /usr/local/bin



You can access individual elements with square brackets. Indexing starts at 1 from the beginning, and -1 from the end::

    >_ echo $PATH
    <outp>/usr/bin /bin /usr/sbin /sbin /usr/local/bin</outp>
    >_ echo $PATH[1]
    <outp>/usr/bin</outp>
    >_ echo $PATH[-1]
    <outp>/usr/local/bin</outp>


You can also access ranges of elements, known as "slices:"



::

    >_ echo $PATH[1..2]
    <outp>/usr/bin /bin</outp>
    >_ echo $PATH[-1..2]
    <outp>/usr/local/bin /sbin /usr/sbin /bin</outp>


You can iterate over a list (or a slice) with a for loop::

    >_ for val in $PATH
        echo "entry: $val"
      end
    <outp>entry: /usr/bin/</outp>
    <outp>entry: /bin</outp>
    <outp>entry: /usr/sbin</outp>
    <outp>entry: /sbin</outp>
    <outp>entry: /usr/local/bin</outp>


Lists adjacent to other lists or strings are expanded as :ref:`cartesian products <cartesian-product>` unless quoted (see :ref:`Variable expansion <expand-variable>`)::

    >_ set a 1 2 3
    >_ set 1 a b c
    >_ echo $a$1
    <outp>1a 2a 3a 1b 2b 3b 1c 2c 3c</outp>
    >_ echo $a" banana"
    <outp>1 banana 2 banana 3 banana</outp>
    >_ echo "$a banana"
    <outp>1 2 3 banana</outp>


This is similar to `Brace expansion <index#expand-brace>`__.

Command Substitutions
---------------------

Command substitutions use the output of one command as an argument to another. Unlike other shells, ``fish`` does not use backticks `` for command substitutions. Instead, it uses parentheses::

    >_ echo In (pwd), running (uname)
    <outp>In /home/tutorial, running FreeBSD</outp>


A common idiom is to capture the output of a command in a variable::

    >_ set os (uname)
    >_ echo $os
    <outp>Linux</outp>


Command substitutions are not expanded within quotes. Instead, you can temporarily close the quotes, add the command substitution, and reopen them, all in the same argument::

    >_ touch <i class="quote">"testing_"</i>(date +%s)<i class="quote">".txt"</i>
    >_ ls *.txt
    <outp>testing_1360099791.txt</outp>


Unlike other shells, fish does not split command substitutions on any whitespace (like spaces or tabs), only newlines. This can be an issue with commands like ``pkg-config`` that print what is meant to be multiple arguments on a single line. To split it on spaces too, use ``string split``.



::

    >_ printf '%s\n' (pkg-config --libs gio-2.0)
    <outp>-lgio-2.0 -lgobject-2.0 -lglib-2.0</outp>
    >_ printf '%s\n' (pkg-config --libs gio-2.0 | string split " ")
    <outp>-lgio-2.0
    -lgobject-2.0
    -lglib-2.0</outp>



Separating Commands (Semicolon)
-------------------------------

Like other shells, fish allows multiple commands either on separate lines or the same line.

To write them on the same line, use the semicolon (";"). That means the following two examples are equivalent::

    echo fish; echo chips
    
    # or
    echo fish
    echo chips



Combiners (And, Or, Not)
------------------------

fish supports the familiar ``&&`` and ``||`` to combine commands, and ``!`` to negate them::

    >_ ./configure && make && sudo make install


fish also supports ``and``, ``or``, and ``not``. The first two are job modifiers and have lower precedence. Example usage::

    >_ cp file1.txt file1_bak.txt && cp file2.txt file2_bak.txt ; and echo "Backup successful"; or echo "Backup failed"
    <outp>Backup failed</outp>


As mentioned in `the section on the semicolon <#tut_semicolon>`__, this can also be written in multiple lines, like so::

    cp file1.txt file1_bak.txt && cp file2.txt file2_bak.txt
    and echo "Backup successful"
    or echo "Backup failed"



Conditionals (If, Else, Switch)
-------------------------------

Use ``if``, ``else if``, and ``else`` to conditionally execute code, based on the exit status of a command.



::

    if grep fish /etc/shells
        echo Found fish
    else if grep bash /etc/shells
        echo Found bash
    else
        echo Got nothing
    end


To compare strings or numbers or check file properties (whether a file exists or is writeable and such), use :ref:`test <cmd-test>`, like



::

    if test "$fish" = "flounder"
        echo FLOUNDER
    end
    
    # or
    
    if test "$number" -gt 5
        echo $number is greater than five
    else
        echo $number is five or less
    end


`Combiners <#tut_combiners>`__ can also be used to make more complex conditions, like



::

    if grep fish /etc/shells; and command -sq fish
        echo fish is installed and configured
    end


For even more complex conditions, use ``begin`` and ``end`` to group parts of them.

There is also a ``switch`` command::

    switch (uname)
    case Linux
        echo Hi Tux!
    case Darwin
        echo Hi Hexley!
    case FreeBSD NetBSD DragonFly
        echo Hi Beastie!
    case '*'
        echo Hi, stranger!
    end


Note that ``case`` does not fall through, and can accept multiple arguments or (quoted) wildcards.


Functions
---------

A ``fish`` function is a list of commands, which may optionally take arguments. Unlike other shells, arguments are not passed in "numbered variables" like ``$1``, but instead in a single list ``$argv``. To create a function, use the ``function`` builtin::

    >_ function say_hello
         echo Hello $argv
      end
    >_ say_hello
    <outp>Hello</outp>
    >_ say_hello everybody!
    <outp>Hello everybody!</outp>


Unlike other shells, ``fish`` does not have aliases or special prompt syntax. Functions take their place.

You can list the names of all functions with the ``functions`` keyword (note the plural!). ``fish`` starts out with a number of functions::

    >_ functions
    <outp>alias, cd, delete-or-exit, dirh, dirs, down-or-search, eval, export, fish_command_not_found_setup, fish_config, fish_default_key_bindings, fish_prompt, fish_right_prompt, fish_sigtrap_handler, fish_update_completions, funced, funcsave, grep, help, history, isatty, ls, man, math, nextd, nextd-or-forward-word, open, popd, prevd, prevd-or-backward-word, prompt_pwd, psub, pushd, seq, setenv, trap, type, umask, up-or-search, vared</outp>


You can see the source for any function by passing its name to ``functions``::

    >_ functions ls
    function ls --description 'List contents of directory'
        command ls -G $argv
    end



Loops
-----

While loops::

    >_ while true
        echo <i class="quote">"Loop forever"</i>
    end
    <outp>Loop forever</outp>
    <outp>Loop forever</outp>
    <outp>Loop forever</outp>
    <outp>...</outp>


For loops can be used to iterate over a list. For example, a list of files::

    >_ for file in *.txt
        cp $file $file.bak
    end


Iterating over a list of numbers can be done with ``seq``::

    >_ for x in (seq 5)
        touch file_$x.txt
    end



Prompt
------

Unlike other shells, there is no prompt variable like PS1. To display your prompt, ``fish`` executes a function with the name ``fish_prompt``, and its output is used as the prompt.

You can define your own prompt::

    >_ function fish_prompt
        echo "New Prompt % "
    end
    <asis>New Prompt % </asis>___


Multiple lines are OK. Colors can be set via ``set_color``, passing it named ANSI colors, or hex RGB values::

    >_ function fish_prompt
          set_color purple
          date "+%m/%d/%y"
          set_color FF0
          echo (pwd) '>'
          set_color normal
      end
    <span style="color: purple">02/06/13</span>
    <span style="color: #FF0">/home/tutorial ></span>___


You can choose among some sample prompts by running ``fish_config prompt``. ``fish`` also supports RPROMPT through ``fish_right_prompt``.

$PATH
-----

``$PATH`` is an environment variable containing the directories in which ``fish`` searches for commands. Unlike other shells, $PATH is a [list](#tut_lists), not a colon-delimited string.

To prepend /usr/local/bin and /usr/sbin to ``$PATH``, you can write::

    >_ set PATH /usr/local/bin /usr/sbin $PATH


To remove /usr/local/bin from ``$PATH``, you can write::

    >_ set PATH (string match -v /usr/local/bin $PATH)


You can do so directly in ``config.fish``, like you might do in other shells with ``.profile``. See :ref:`this example <path_example>`.

A faster way is to modify the ``$fish_user_paths`` [universal variable](#tut_universal), which is automatically prepended to ``$PATH``. For example, to permanently add ``/usr/local/bin`` to your ``$PATH``, you could write::

    >_ set -U fish_user_paths /usr/local/bin $fish_user_paths


The advantage is that you don't have to go mucking around in files: just run this once at the command line, and it will affect the current session and all future instances too. (Note: you should NOT add this line to ``config.fish``. If you do, the variable will get longer each time you run fish!)

Startup (Where's .bashrc?)
--------------------------

``fish`` starts by executing commands in ``~/.config/fish/config.fish``. You can create it if it does not exist.

It is possible to directly create functions and variables in ``config.fish`` file, using the commands shown above. For example:

.. _path_example:

::

    >_ cat ~/.config/fish/config.fish
    
    set -x PATH $PATH /sbin/
    
    function ll
        ls -lh $argv
    end


However, it is more common and efficient to use  autoloading functions and universal variables.

Autoloading Functions
---------------------

When ``fish`` encounters a command, it attempts to autoload a function for that command, by looking for a file with the name of that command in ``~/.config/fish/functions/``.

For example, if you wanted to have a function ``ll``, you would add a text file ``ll.fish`` to ``~/.config/fish/functions``::

    >_ cat ~/.config/fish/functions/ll.fish
    function ll
        ls -lh $argv
    end


This is the preferred way to define your prompt as well::

    >_ cat ~/.config/fish/functions/fish_prompt.fish
    function fish_prompt
        echo (pwd) "> "
    end


See the documentation for :ref:`funced <cmd-funced>` and :ref:`funcsave <cmd-funcsave>` for ways to create these files automatically.

Universal Variables
-------------------

A universal variable is a variable whose value is shared across all instances of ``fish``, now and in the future – even after a reboot. You can make a variable universal with ``set -U``::

    >_ set -U EDITOR vim


Now in another shell::

    >_ echo $EDITOR
    vim


.. _switching-to-fish:

Switching to fish?
------------------

If you wish to use fish (or any other shell) as your default shell,
you need to enter your new shell's executable ``/usr/local/bin/fish`` in two places:
- add ``/usr/local/bin/fish`` to ``/etc/shells``
- change your default shell with ``chsh -s /usr/local/bin/fish``

You can use the following commands for this:

Add the fish shell ``/usr/local/bin/fish``
to ``/etc/shells`` with::

    >echo /usr/local/bin/fish | sudo tee -a /etc/shells


Change your default shell to fish with::

    >chsh -s /usr/local/bin/fish


(To change it back to another shell, just substitute ``/usr/local/bin/fish``
with ``/bin/bash``, ``/bin/tcsh`` or ``/bin/zsh`` as appropriate in the steps above.)


Ready for more?
---------------

If you want to learn more about fish, there is :ref:`lots of detailed documentation <intro>`, an `official mailing list <https://lists.sourceforge.net/lists/listinfo/fish-users>`__, the IRC channel \#fish on ``irc.oftc.net``, and the `github page <https://github.com/fish-shell/fish-shell/>`__.
