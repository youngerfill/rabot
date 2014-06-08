Rabot
=====

.. contents::
    :local:
    :backlinks: none

Introduction
------------

Rabot (an acronym for "**R**\ abot: a **b**\ unch **o**\ f **t**\ ools") is a collection of bash scripts.

They can be installed simply by downloading them and adding the ``rabot`` directory to the ``PATH`` variable in your ``.bashrc`` file. There's a script called ``addtree2path`` in ``rabot`` to do just that.
If you add the following line to ``.bashrc``:

.. code-block:: bash

    . ~/tools/rabot/addtree2path ~/tools

then the entire ``tools`` directory tree (including ``rabot``) gets added to the ``PATH`` variable.

Every executable script in ``rabot`` has its own help function. You get the help by typing ``scriptname --help``. There are only a few non-executable scripts: ``addtree2path`` and a couple of scripts that are only meant to be used internally by other scripts.

The rest of this document consists of the ``--help`` output of the scripts.

bu_this
-------

.. code-block:: bash

  Usage: bu_this

  Summary: Compress the contents of the Current Working Directory (CWD)
           into a .tgz file saved in a dedicated backup directory.

  'bu_this' performs the following actions:

  1. Obtain the path to the destination directory by invoking 'rabot_vars backupDir'
     and create this directory if it doesn't already exist.

  2. Run the script 'tgz_this' to compress the contents of the CWD into a .tgz file
     located in the parent directory of the CWD.

  3. 'mv' this .tgz file to the destination directory.

clean_this
----------

.. code-block:: bash

  Usage: clean_this

  'clean_this' performs the following actions:

  1. Check for an executable file named 'clean' in the
     Current Working Directory (CWD) and run it, if found.

  2. Check for a file named 'clean.txt' in the CWD and
     - if found - delete all files and directories named in
     this file.

  3. Check for a file named 'Makefile' in the CWD and
     - if found - run the command 'make clean'.

  4. Check for subdirectories of the CWD matching zz_*
     and delete them - if found. Only first-level subdirs are
     considered, matching directories at deeper levels are
     left untouched.

flon
----

.. code-block:: bash

  Usage: FINDCOMMAND | flon

  'flon' takes the output of a 'find' command
  (or a command with similar output) and opens it in the
  'quickfix' window of a 'vim' session.

  This allows for easy navigation through all files found
  by FINDCOMMAND.

  The 'quickfix' window will assume that the output
  contains nothing but filenames, as 'vim' will be
  started with 'errorformat' equal to '%f'.

  Example:

      find . -type f | flon

  This will open 'vim' and display the 'quickfix' window.
  The latter window will contain a list of every file in
  the current working directory and all its subdirectories.

fnd
---

.. code-block:: bash

  Usage: fnd ['find' arguments]

  'fnd' wraps the 'find' tool by adding options that make it
  exclude directories with the following names:

      '.git', '.hg', '.svn', '.bzr' and 'CVS'

fnd0
----

.. code-block:: bash

  Usage: fnd0 ['find' arguments]

  'fnd0' is similar to 'fnd' but adds a '-print0' option to
  the 'find' command.

  For more info, see 'fnd --help'.

fullts
------

.. code-block:: bash

  Usage: fullts [FILE]

  'fullts' displays the current time in the format:
  'YYYMMDDhhmmss'. If the argument FILE is given, it displays
  the timestamp of FILE in this format.

glon
----

.. code-block:: bash

  Usage: GREPCOMMAND | glon

  'glon' takes the output of a 'grep' command
  (or a command with similar output) and opens it in the
  'quickfix' window of a 'vim' session.

  This allows for easy navigation through all matching
  lines found by GREPCOMMAND.

  The 'quickfix' window will assume the following format
  for the output lines:

      '%f:%l:%m'

  where '%f' is the filename, '%l' is the linenumber and
  '%m' is the rest of the line.

  If 'grep' is used as the command, the option '-n' must
  be used in order to produce this format.

  Example:

       grp rabot . | glon

  This makes use of the 'grep' wrapper script called 'grp'.
  Vim will be started and the quickfix window will be
  displayed, containing a list of all occurences of the
  search term 'rabot' found in files of the current working
  directory and its subdirectories.

grp
---

.. code-block:: bash

  Usage: grp [OPTIONS] REGEX DIRECTORY

  'grp' wraps the 'grep' tool by adding the options: '-nrIP'.

  This means, respectively: display line numbers, search recursively
  through the directory tree, skip binary files and use the PCRE regex
  flavor.

  Additionally, directories named '.git', '.hg', '.svn', '.bzr' or 'CVS'
  will be skipped during the search, and output will be displayed in
  colour.

logop
-----

.. code-block:: bash

  Usage:
      first form:
          logop COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logop

  In the first form, 'logop' invokes the command string and sends
  its output (both stdout and stderr) to two different targets: stdout and a
  logfile.

  In the second form, the stdout of the command is piped to 'logop',
  where it is duplicated over stdout and a logfile. If you want to log stderr
  too, redirect it to stdout first, like this:

      COMMAND [ARG1]... 2>&1 | logop

  In addition to passing on the output of the command, 'logop'
  adds a header and a footer section with supplementary information. If the
  second form is used however, this information will not contain the command
  string that has been invoked nor the exit status of the command.

  The logfile is saved in the folder obtained from invoking 'rabot_vars logDir'.
  The filename of the logfile has the following form:

      YYYYMMDDhhmmss_RND.txt

  The part before the extension is the current time and a random alphanumerical
  string, as explained in 'timestamp_id --help'.

  In the log directory a symbolic link called 'latest' will be created or updated
  pointing to the newly created logfile.

  Examples:

  A minimal sample of the first form:

      user@host ~ $ logop echo Hello
      ==== Start log: 2014-05-23 22:31:09
      ==== Logscript: /home/user/tools/rabot/logop/logop
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/log/20140523223109_f4w.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:31:09

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logop
      ==== Start log: 2014-05-23 22:34:24
      ==== Logscript: /home/user/tools/rabot/logop/logop
      ==== Working directory: /home/user
      ==== Logfile: /home/user/log/20140523223423_q5n.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:34:24

logopd
------

.. code-block:: bash

  Usage:
      first form:
          logopd DIR COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logopd DIR

  The behavior of 'logopd' is similar to 'logop', with the
  following differences:

  - An extra 'DIR' argument will override the value provided by
    'rabot_vars logDir'.

  - The symlink called 'latest.txt' in the default log directory will
    not be updated. Instead, a 'latest.txt' symlink is created/updated
    in the 'DIR' directory.

  For further info, see: 'logop --help'

  A minimal sample of the first form:

      user@host ~ $ logopd mylogdir echo Hello
      ==== Start log: 2014-05-23 22:37:40
      ==== Logscript: /home/user/tools/rabot/logop/logopd
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogdir/20140523223740_8yo.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:37:40

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logopd mylogdir
      ==== Start log: 2014-05-23 22:38:17
      ==== Logscript: /home/user/tools/rabot/logop/logopd
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogdir/20140523223817_0r0.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:38:17

logopf
------

.. code-block:: bash

  Usage:
      first form:
          logopf FILE COMMAND [ARG1]...

      second form:
          COMMAND [ARG1]... | logopf FILE

  The behavior of 'logopf' is similar to 'logop', with the
  following differences:

  - An extra 'FILE' argument specifies the logfile. 'logopf'
    never deletes the contents of this file but only appends to it.

  - No symlink 'latest.txt' is created or updated.

  For further info, see: 'logop --help'

  A minimal sample of the first form:

      user@host ~ $ logopf mylogfile.txt echo Hello
      ==== Start log: 2014-05-23 22:43:03
      ==== Logscript: /home/user/tools/rabot/logop/logopf
      ==== Command: echo Hello
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogfile.txt

      Hello

      ==== Exit status: 0
      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:43:03

  A minimal sample of the second form:

      user@host ~ $ echo Hello | logopf mylogfile.txt
      ==== Start log: 2014-05-23 22:43:18
      ==== Logscript: /home/user/tools/rabot/logop/logopf
      ==== Working directory: /home/user
      ==== Logfile: /home/user/mylogfile.txt

      Hello

      ==== Elapsed: 0.00 seconds
      ==== End log: 2014-05-23 22:43:18

rabot_vars
----------

.. code-block:: bash

  Usage: rabot_vars VARNAME

  'rabot_vars' collects some configuration settings of 'rabot'.

  It will output the value of the variable whose name is specified
  as a command-line argument.

  These values can be overridden outside 'rabot_vars' by redefining
  the variable before calling this script. For example:

      $ rabot_vars logDir
      MyNormalLogDir
      $ export logDir=MySpecialLogDir
      $ rabot_vars logDir
      MySpecialLogDir

  For a list of all variables defined by 'rabot_vars' and
  their values, see the source code of the script.

randid
------

.. code-block:: bash

  Usage: randid [LENGTH]

  'randid' prints a random alphanumerical string of
  LENGTH characters (3 by default).

  Example:

      user@host ~ $ randid 5
      mx2ft

tgz_files
---------

.. code-block:: bash

  Usage: tgz_files FILELIST DESTDIR [PREFIX]

  'tgz_files' reads the file FILELIST and creates a .tgz file
  (with the command 'tar') containing all files and directories
  listed in FILELIST.

  FILELIST must contain one path to a file or directory per line.
  Paths can be either absolute or relative to the current working
  directory.

  If a path starts with '~', the tilde will be
  replaced with the value of $HOME (on this system: /home/wezzel)
  before being passed to 'tar'.

  Inside the created .tgz file, all paths will be absolute, even
  the paths that were relative in the FILELIST.

  The directory DESTDIR will be created if it does not exist.

  The name of the destination file will be in the format:
      YYYYMMDDhhmmss_rnd.tgz
  where 'YYYYMMDDhhmmss' is the creation time of the .tgz file
  and 'rnd' is a random 3-character string consisting of numerals
  and/or lowercase letters. If a third argument 'PREFIX' is
  specified, the filename will be:
      PREFIX_YYYYMMDDhhmmss_rnd.tgz

  Example:

  With a file 'filelist.txt' containing the following two lines:
      one.txt
      two.txt

  The command and its output look like this:
      user@host ~ $ tgz_files filelist.txt .
      /home/user/one.txt
      /home/user/two.txt
      /home/user/20140519142819_5sp.tgz

tgz_folder
----------

.. code-block:: bash

  Usage: tgz_folder FOLDER DESTDIR [PREFIX]

  'tgz_folder' compresses the directory FOLDER to a .tgz file and saves
  the latter in the directory DESTDIR.

  The filename has the following pattern:

      'NAME_YYYMMDDhhmmss_RND.tgz'

  where 'NAME' is either equal to the name of 'FOLDER' or to 'PREFIX' if the
  latter argument is given, 'YYYMMDDhhmmss' is the current datetime and 'RND'
  is a 3-character random alphanumerical string.

  The directory DESTDIR will be created if it does not exist.

  Paths inside the .tgz file will be relative to the current working directory.

  Example:

      user@host ~ $ tgz_folder somedir/myfolder .
      /home/user/myfolder_20140522224511_fw0.tgz

tgz_this
--------

.. code-block:: bash

  Usage: tgz_this

  Compress the contents of the Current Working Directory (CWD)
  to a .tgz file stored in its parent directory.

  'tgz_this' performs the following actions:

  1. Remove temporary files from the CWD by running the script
     'clean_this'.

  2. 'cd' into the parent directory of the CWD and run
     the script 'tgz_folder' on the former CWD.

  Example:

      user@host ~/myfolder $ tgz_this
      /home/user/myfolder_20140522221601_5ve.tgz

timestamp_id
------------

.. code-block:: bash

  Usage: timestamp_id

  'timestamp_id' will print the current time plus a
  3-character random alphanumerical string in the following way:

      YYYYMMDDhhmmss_RND

  where 'YYYYMMDDhhmmss' is the timestamp (produced by 'fullts')
  and 'RND' is the random string (produced by 'randid').

  Example:

      user@host ~ $ timestamp_id
      20140328133629_1oy

walkdir
-------

.. code-block:: bash

  Usage: walkdir COMMAND [ARG1]...

  'walkdir' performs COMMAND with its arguments in
  every directory of the tree rooted in the current working
  directory.

  Example:

      user@host ~ $ walkdir pwd
      /home/user
      /home/user/mydir
      /home/user/myotherdir

walkdird
--------

.. code-block:: bash

  Usage: walkdird DIR COMMAND [ARG1]...

  'walkdird' performs COMMAND with its arguments in
  every directory of the tree rooted in DIR.

  Example:

      user@host / $ walkdird ~ pwd
      /home/user
      /home/user/mydir
      /home/user/myotherdir

zip_folder
----------

.. code-block:: bash

  Usage: zip_folder FOLDER DESTDIR [PREFIX]

  'zip_folder' compresses the directory FOLDER to a .zip file and saves
  the latter in the directory DESTDIR.

  The filename has the following pattern:

      'NAME_YYYMMDDhhmmss_RND.zip'

  where 'NAME' is either equal to the name of 'FOLDER' or to 'PREFIX' if the
  latter argument is given, 'YYYMMDDhhmmss' is the current datetime and 'RND'
  is a 3-character random alphanumerical string.

  The directory DESTDIR will be created if it does not exist.

  Paths inside the .zip file will be relative to the current working directory.

  Example:

      user@host ~ $ zip_folder somedir/myfolder .
      /home/user/myfolder_20140522224809_m94.zip

zip_this
--------

.. code-block:: bash

  Usage: zip_this

  Compress the contents of the Current Working Directory (CWD)
  to a .zip file stored in its parent directory.

  'zip_this' performs the following actions:

  1. Remove temporary files from the CWD by running the script
     'clean_this'.

  2. 'cd' into the parent directory of the CWD and run
     the script 'zip_folder' on the former CWD.

  Example:

      user@host ~/myfolder $ zip_this
      /home/user/myfolder_20140522225226_0fg.zip
