flask-paginate |release| documentation
===========================================

.. module:: flask.ext.paginate

Overview
---------
Latest version: **0.2.8**

**flask-paginate** is a simple paginate extension for
`flask`_ which is reference to `will_paginate`_,
and use `bootstrap`_ as css framework.

**0.2 update**: `foundation`_ is now supported (use **css_framework** parameter)::

    pagination = Pagination(..., css_framework='foundation', **kwargs)

**Note**: Python 2.6+ is required (**string.format** is used to format text)

.. sourcecode:: html+jinja

    {{ pagination.info }}

.. figure:: _static/paginate-info.png
    :alt: The screenshot of paginate information

or

.. figure:: _static/paginate-info2.png
    :alt: The screenshot of paginate information when search

.. sourcecode:: html+jinja

    {{ pagination.links }}

.. figure:: _static/paginate-links.png
    :alt: The screenshot of paginate links

.. highlight:: bash

Installation
------------------------

Install the extension with one of the following commands::

  $ easy_install -U flask-paginate

or alternatively if you have pip installed::

  $ pip install -U flask-paginate

Configuration
------------------

If you want to show the pagination information, add below lines to your
css file.

.. sourcecode:: css

    .pagination-page-info {
        padding: .6em;
        padding-left: 0;
        width: 40em;
        margin: .5em;
        margin-left: 0;
        font-size: 12px;
    }
    .pagination-page-info b {
        color: black;
        background: #6aa6ed;
        padding-left: 2px;
        padding: .1em .25em;
        font-size: 150%;
    }

How to use
------------
In your flask views file (e.g. views/users.py)::

    from flask import Blueprint
    from flask.ext.paginate import Pagination

    mod = Blueprint('users', __name__)


    @mod.route('/')
    def index():
        search = False
        q = request.args.get('q')
        if q:
            search = True
        try:
            page = int(request.args.get('page', 1))
        except ValueError:
            page = 1

        users = User.find(...)
        pagination = Pagination(page=page, total=users.count(), search=search, record_name='users')
        return render_template('users/index.html',
                               users=users,
                               pagination=pagination,
                               )

In the **users/index.html**:

.. sourcecode:: html+jinja

    {{ pagination.info }}
    {{ pagination.links }}
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        {% for user in users %}
          <tr>
            <td>{{ loop.index + users.skip }}</td>
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
          </tr>
        {% endfor %}
      </tbody>
    </table>
    {{ pagination.links }}


Example
--------

.. sourcecode:: sh

    $cd example
    $python sql.py
    $python sql.py init_db
    $python sql.py fill_data
    $cp app.cfg.example app.cfg
    $echo edit app.cfg
    $python app.py

Open `<http://localhost:5000>`_ to see the example page.

.. figure:: _static/demo.png
    :alt: The screenshot of paginate example

Deep into the Pagination
--------------------------

Below are the params for **Pagination.__init__()**, you can change the settings here.

    **found**: used when searching

    **page**: current page

    **per_page**: how many items displayed on one page

    **inner_window**: how many links arround current page

    **outer_window**: how many links near first/last link

    **prev_label**: text for previous page, default is **&laquo;**

    **next_label**: text for next page, default is **&raquo;**

    **search**: search or not?

    **total**: total records for pagination

    **display_msg**: text for pagation information

    **search_msg**: text for search information

    **record_name**: record name showed in pagination information

    **link_size**: font size of page links

    **alignment**: the alignment of pagination links

    **href**: Add custom href for links - this supports forms with post method

    **show_single_page**: decide whether or not a single page returns pagination

    **bs_version**: the version of bootstrap, default is **2**

    **css_framework**: the css framework, default is **bootstrap**, you can use it instead of **bs_version**

API
------------------

.. autoclass:: Pagination
   :members:

.. toctree::
   :maxdepth: 2

Changelog
---------

Version 0.2.8
-------------

(bugfix release, released on Nov 2nd 2014)

- Flat args for non-multi values style urls. This is used to fix the bug
  `<https://github.com/lixxu/flask-paginate/issues/24>`_
  when generated url likes this (`http://xxxxxx/something/[u'param']?page=2`)
- Use `<http://flask.pocoo.org/docs/0.10/api/#flask.Markup>`_ when generating
  url so that template no need to use `safe` filter any more.

.. _Flask: http://flask.pocoo.org/
.. _will_paginate: https://github.com/mislav/will_paginate/wiki
.. _bootstrap: http://twitter.github.com/bootstrap/
.. _foundation: http://foundation.zurb.com/


