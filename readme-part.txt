Rabot
=====

.. contents::
    :local:
    :backlinks: none

Introduction
------------

Rabot (an acronym for "**R**\ abot: **a** **b**\ unch **o**\ f **t**\ ools") is a collection of utility bash scripts.

They can be installed simply by downloading them and adding the ``rabot`` directory to the ``PATH`` variable in your ``.bashrc`` file. There's a script called ``addtree2path`` in ``rabot`` to do just that.
For example, if you add the following line to ``.bashrc``:

::

    . ~/tools/rabot/addtree2path ~/tools

then the entire ``tools`` directory tree (including ``rabot``) gets added to the ``PATH`` variable.

Every executable script in ``rabot`` has its own help function. You get the help by typing ``scriptname --help``. There are only a few non-executable scripts: ``addtree2path`` and a couple of scripts that are only meant to be used internally by other scripts.

Most scripts fall into the following categories:

* Easily create compressed, timestamped snapshots of your work-in-progress: ``bu-this``, ``clean-this``, ``tgz-files``, ``tgz-folder``, ``tgz-this``, ``zip-folder``, ``zip-this``. This can come in handy if you are collaborating with someone over email, or for little things where you think proper version control is too much hassle.
* Save the output of console programs to logfiles: ``logop``, ``logopd``, ``logopf``.
* Search through directory trees while ignoring the subdirectories created by version control software: ``fnd``, ``fnd0``, ``grp``.
* Navigate through search results with ``vim`` and its ``quickfix`` window: ``flon``, ``glon``.

The rest of this document consists of the ``--help`` output of the scripts.
