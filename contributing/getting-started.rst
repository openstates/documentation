Getting Started
===============

No matter how experienced you are, it is a good idea to read this section before diving into Open States' code.

This guide assumes a basic familiarity with using the command line, git, and Python.

No worries if you aren't an expert though, we'll walk you through the steps.  And as for Python, if you've written other languages like Javascript or Ruby you'll probably be just fine.  Don't be afraid to :ref:`ask for help <getting-help>` either!

.. _prerequisites:

Installing docker
-----------------

The first thing you will need to do is get a working docker environment on your local machine.  We'll do this using Docker.  No worries if you aren't familiar with Docker, you'll barely have to touch it beyond what this guide explains.

Install Docker and docker-compose (if not already installed on your local system):

  **(a)** Installing Docker:

  * On OSX: `Docker for Mac <https://docs.docker.com/docker-for-mac/>`_
  * On Windows: `Docker for Windows <https://docs.docker.com/docker-for-windows/>`_
  * On Linux: Use your package manager of choice or `follow Docker's instructions <https://docs.docker.com/engine/installation/linux/>`_.        
  
  (*Docker Compose is probably already installed by step 1(a) if not, proceed to step 1(b)*) 
  
  **(b)** Installing docker-compose:

  * For easy installation on `macOS, Windows, and 64-bit Linux. <https://docs.docker.com/compose/install/#prerequisites>`_
    
Ensure that Docker and docker-compose are installed locally::

    $ docker --version
    Docker version 19.03.4, build 9013bf5
    $ docker-compose --version
    docker-compose version 1.24.1, build 4667896b

Of course, your versions will differ, but ensure they are relatively recent to avoid strange issues.

.. _pre-commit:

Installing pre-commit
---------------------

To help keep the code as managable as possible we **strongly recommend** you use pre-commit to make sure all commits adhere to our preferred style.

  * See `pre-commit's installation instructions <https://pre-commit.com/#installation>`_
  * Within each repo you check out, run ``pre-commit install`` after checking out. It should look something like::

      $ pre-commit install
      pre-commit installed at .git/hooks/pre-commit

.. note::
  If you're running ``flake8`` and ``black`` yourself via your editor or similar this isn't strictly necessary, but we find it helps ensure commits don't fail linting.  **We require all PRs to pass linting!**


Get Familiar With Our Processes
-------------------------------

We're glad to have you joining us, taking a few minutes to read the following pages will help you be a better member of our community:

* Our :ref:`Code of Conduct <code-of-conduct>` is important to us, and helps us maintain a healthy community.
* We also have a :ref:`guide to help you learn where to get help <communication>` that you should look over.
* There's also a :ref:`repository overview and roadmap <overview>` that can be helpful context for the work we're doing here.
