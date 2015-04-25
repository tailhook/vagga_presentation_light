:css: my.css
:js: my.js

.. title:: vagga

----

:id: title

=====
Vagga
=====

----

Vagga
=====

* simple YAML config (+versioning)
* user namespaces (no root/setuid)
* multiple process monitoring
* only for dev.env.

----

.. code-block:: yaml

    # vagga.yaml
    containers:
      "react":
        setup:
        - !NpmInstall [react-tools]
    commands:
      "build": !Command
        container: react
        description: "Build static files"
        run: "jsx jsx/page.jsx > public/js/page.js"

----

.. code-block:: console

    $ git clone git://github.com/.../foobar
    $ cd foobar
    $ vagga
    Available commands:
        build       Build static files
        run         Run nginx+app+redis
        build-docs  Build docs
    $ vagga build

----

.. code-block:: console

    $ git pull
    $ vagga run

----

::

   # docker tree
   -+= 00001 root systemd --system
    |-+- 10771 root docker -d
    | \--= 32029 root bash   << our process
    \-+= 30029 pc tmux
      \-+= 10718 pc -zsh     << our shell
        \--= 32021 pc docker run -it --rm bash

::

   # vagga tree
   -+= 00001 root systemd --system
    \-+= 30029 pc tmux
      \-+= 10358 pc -zsh        << our shell
        \-+= 00940 pc vagga bash
          \-+- 00941 pc vagga bash
            \--= 00942 pc bash  << our process

----

.. code-block:: yaml

    # vagga.yaml
    containers:
      "py":
        setup:
        - !Ubuntu trusty
        - !BuildDeps [base-devel, cython]
        - !PipConfig { dependencies: true }
        - !Py3Requirements requirements.txt


----


.. code-block:: yaml

  run: !Supervise
    description: Run full server stack
    children:
      redis: !Command
        container: redis
        run: [redis-server, --daemonize, no]
      nginx: !Command
        container: nginx
        run: [nginx, -c, /work/config/nginx.conf]
      foobar: !Command
        run: [python, -m, foobar]

----

Vagga TODO
==========

* pip-accel (cache binary packages)
* flake8 (i.e. project-specific style checks)

----

.. image:: vagga.svg
   :width: 500

* http://github.com/tailhook/vagga
* http://vagga.readthedocs.org

