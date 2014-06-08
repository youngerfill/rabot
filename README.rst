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

.. include:: allhelp.rst
