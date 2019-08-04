.. _theming-pelican:

创建主题
###############

要生成其HTML输出，Pelican使用具有灵活性和语法简单等特点的 `Jinja <http://jinja.pocoo.org/>`_ 模
板引擎。如果你想创建自己的主题，欢迎随时从 `"simple" theme
<https://github.com/getpelican/pelican/tree/master/pelican/themes/simple/templates>`_ 主题中汲取灵感

要使用你创建的（或手动下载后修改的）主题生成网站，可以通过在pelican命令中加 ``-t`` 参数来指定该主题::

    pelican content -s pelicanconf.py -t /projects/your-site/themes/your-theme

如果你不希望在每个调用上指定主题，则可以在 ``THEME`` 变量中设置指向首选主题的位置。


主题框架
=========

要制作你自己的主题，你必须遵循以下的文件结构::

    ├── static
    │   ├── css
    │   └── images
    └── templates
        ├── archives.html         // 显示存档
        ├── period_archives.html  // 显示时间段存档
        ├── article.html          // 应用于每篇文章
        ├── author.html           // 应用于每个作者
        ├── authors.html          // 应用于所有作者
        ├── categories.html       // 列出所有分类
        ├── category.html         // 应用于每个分类
        ├── index.html            // 索引 (列出所有文章)
        ├── page.html             // 应用于每个page页
        ├── tag.html              // 应用于每个标签
        └── tags.html             // 列出所有标签，可以是标签云

* `static` 包含所有静态资源，这些静态资源将被复制到输出目录的 `theme` 文件夹。
  上述文件系统布局中包括CSS文件夹和图像文件夹，但这些只是示例。把你需要的放在这个目录下。

* `templates` 包含将用于生成内容的所有模板。上面列出的模板文件是强制性的；
  创建主题时如果有需要，你可以添加自己的模板文件。


.. _templates-variables:

模板和变量
=======================

其理念是让你可以使用简单的语法嵌入到 HTML 页面中。
本文档描述的是主题中应存在哪些模板，生成站点时会将哪些变量传递给每个模板。

只要设置文件中定义的变量拼写全大写，所有模板都将接收它们。你可以直接访问它们。


常用变量
----------------

所有这些设置将适用于所有模板。

=============   ===================================================
变量             描述
=============   ===================================================
output_file     当前正在生成的文件的名称。例如，当pelican渲染主页，输出
                文件output_file将为 "index.html" 。
articles        文章列表，按日期降序排序。 所有元素都是 `Article` 文章对象，
                因此你可以访问它们的属性（例如标题、摘要、作者等）。有时
                这些信息被隐去（比如在标签页）。不过你可以在所有文章 
                 `all_articles` 变量中找到有关的信息。
dates           同样是文章列表，不过按日期升序排序。
drafts          文章草稿列表。
authors         一个元组tuples（作者，文章）列表，包含所有作者和相应的文章（值）。
categories      一个元组tuples（分类，文章）列表，包含所有分类和相应的文章（值 ）。
tags            一个元组tuples（标签，文章）列表，包含所有标签和相应的文章（值 ）。
pages           pages页面列表
hidden_pages    隐藏的pages页面列表
draft_pages     pages页面草稿列表
=============   ===================================================


排序
-------

URL wrappers (currently categories, tags, and authors), have comparison methods
that allow them to be easily sorted by name::

    {% for tag, articles in tags|sort %}

If you want to sort based on different criteria, `Jinja's sort command`__ has a
number of options.

__ http://jinja.pocoo.org/docs/templates/#sort


Date Formatting
---------------

Pelican formats the date according to your settings and locale
(``DATE_FORMATS``/``DEFAULT_DATE_FORMAT``) and provides a ``locale_date``
attribute. On the other hand, the ``date`` attribute will be a `datetime`_
object. If you need custom formatting for a date different than your settings,
use the Jinja filter ``strftime`` that comes with Pelican. Usage is same as
Python `strftime`_ format, but the filter will do the right thing and format
your date according to the locale given in your settings::

    {{ article.date|strftime('%d %B %Y') }}

.. _datetime: https://docs.python.org/2/library/datetime.html#datetime-objects
.. _strftime: https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior


index.html
----------

This is the home page or index of your blog, generated at ``index.html``.

If pagination is active, subsequent pages will reside in
``index{number}.html``.

======================  ===================================================
Variable                Description
======================  ===================================================
articles_paginator      A paginator object for the list of articles
articles_page           The current page of articles
articles_previous_page  The previous page of articles (``None`` if page does
                        not exist)
articles_next_page      The next page of articles (``None`` if page does
                        not exist)
dates_paginator         A paginator object for the article list, ordered by
                        date, ascending.
dates_page              The current page of articles, ordered by date,
                        ascending.
dates_previous_page     The previous page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
dates_next_page         The next page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
page_name               'index' -- useful for pagination links
======================  ===================================================


author.html
-------------

This template will be processed for each of the existing authors, with output
generated according to the ``AUTHOR_SAVE_AS`` setting (`Default:`
``author/{slug}.html``). If pagination is active, subsequent pages will by
default reside at ``author/{slug}{number}.html``.

======================  ===================================================
Variable                Description
======================  ===================================================
author                  The name of the author being processed
articles                Articles by this author
dates                   Articles by this author, but ordered by date,
                        ascending
articles_paginator      A paginator object for the list of articles
articles_page           The current page of articles
articles_previous_page  The previous page of articles (``None`` if page does
                        not exist)
articles_next_page      The next page of articles (``None`` if page does
                        not exist)
dates_paginator         A paginator object for the article list, ordered by
                        date, ascending.
dates_page              The current page of articles, ordered by date,
                        ascending.
dates_previous_page     The previous page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
dates_next_page         The next page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
page_name               AUTHOR_URL where everything after `{slug}` is
                        removed -- useful for pagination links
======================  ===================================================


category.html
-------------

This template will be processed for each of the existing categories, with
output generated according to the ``CATEGORY_SAVE_AS`` setting (`Default:`
``category/{slug}.html``). If pagination is active, subsequent pages will by
default reside at ``category/{slug}{number}.html``.

======================  ===================================================
Variable                Description
======================  ===================================================
category                The name of the category being processed
articles                Articles for this category
dates                   Articles for this category, but ordered by date,
                        ascending
articles_paginator      A paginator object for the list of articles
articles_page           The current page of articles
articles_previous_page  The previous page of articles (``None`` if page does
                        not exist)
articles_next_page      The next page of articles (``None`` if page does
                        not exist)
dates_paginator         A paginator object for the list of articles,
                        ordered by date, ascending
dates_page              The current page of articles, ordered by date,
                        ascending
dates_previous_page     The previous page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
dates_next_page         The next page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
page_name               CATEGORY_URL where everything after `{slug}` is
                        removed -- useful for pagination links
======================  ===================================================


article.html
-------------

This template will be processed for each article, with output generated
according to the ``ARTICLE_SAVE_AS`` setting (`Default:` ``{slug}.html``). The
following variables are available when rendering.

=============   ===================================================
Variable        Description
=============   ===================================================
article         The article object to be displayed
category        The name of the category for the current article
=============   ===================================================

Any metadata that you put in the header of the article source file will be
available as fields on the ``article`` object. The field name will be the same
as the name of the metadata field, except in all-lowercase characters.

For example, you could add a field called `FacebookImage` to your article
metadata, as shown below:

.. code-block:: markdown

    Title: I love Python more than music
    Date: 2013-11-06 10:06
    Tags: personal, python
    Category: Tech
    Slug: python-je-l-aime-a-mourir
    Author: Francis Cabrel
    FacebookImage: http://franciscabrel.com/images/pythonlove.png

This new metadata will be made available as `article.facebookimage` in your
`article.html` template. This would allow you, for example, to specify an image
for the Facebook open graph tags that will change for each article:

.. code-block:: html+jinja

    <meta property="og:image" content="{{ article.facebookimage }}"/>


page.html
---------

This template will be processed for each page, with output generated according
to the ``PAGE_SAVE_AS`` setting (`Default:` ``pages/{slug}.html``). The
following variables are available when rendering.

=============   ===================================================
Variable        Description
=============   ===================================================
page            The page object to be displayed. You can access its
                title, slug, and content.
=============   ===================================================


tag.html
--------

This template will be processed for each tag, with output generated according
to the ``TAG_SAVE_AS`` setting (`Default:` ``tag/{slug}.html``). If pagination
is active, subsequent pages will by default reside at
``tag/{slug}{number}.html``.

======================  ===================================================
Variable                Description
======================  ===================================================
tag                     The name of the tag being processed
articles                Articles related to this tag
dates                   Articles related to this tag, but ordered by date,
                        ascending
articles_paginator      A paginator object for the list of articles
articles_page           The current page of articles
articles_previous_page  The previous page of articles (``None`` if page does
                        not exist)
articles_next_page      The next page of articles (``None`` if page does
                        not exist)
dates_paginator         A paginator object for the list of articles,
                        ordered by date, ascending
dates_page              The current page of articles, ordered by date,
                        ascending
dates_previous_page     The previous page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
dates_next_page         The next page of articles, ordered by date,
                        ascending (``None`` if page does not exist)
page_name               TAG_URL where everything after `{slug}` is removed
                        -- useful for pagination links
======================  ===================================================


period_archives.html
--------------------

This template will be processed for each year of your posts if a path for
``YEAR_ARCHIVE_SAVE_AS`` is defined, each month if ``MONTH_ARCHIVE_SAVE_AS`` is
defined, and each day if ``DAY_ARCHIVE_SAVE_AS`` is defined.

===================     ===================================================
Variable                Description
===================     ===================================================
period                  A tuple of the form (`year`, `month`, `day`) that
                        indicates the current time period. `year` and `day`
                        are numbers while `month` is a string. This tuple
                        only contains `year` if the time period is a
                        given year. It contains both `year` and `month`
                        if the time period is over years and months and
                        so on.

===================     ===================================================

You can see an example of how to use `period` in the `"simple" theme
period_archives.html template
<https://github.com/getpelican/pelican/blob/master/pelican/themes/simple/templates/period_archives.html>`_.


Objects
=======

Detail objects attributes that are available and useful in templates. Not all
attributes are listed here, this is a selection of attributes considered useful
in a template.

.. _object-article:

Article
-------

The string representation of an Article is the `source_path` attribute.

======================  ===================================================
Attribute               Description
======================  ===================================================
author                  The :ref:`Author <object-author_cat_tag>` of
                        this article.
authors                 A list of :ref:`Authors <object-author_cat_tag>`
                        of this article.
category                The :ref:`Category <object-author_cat_tag>`
                        of this article.
content                 The rendered content of the article.
date                    Datetime object representing the article date.
date_format             Either default date format or locale date format.
default_template        Default template name.
in_default_lang         Boolean representing if the article is written
                        in the default language.
lang                    Language of the article.
locale_date             Date formatted by the `date_format`.
metadata                Article header metadata `dict`.
save_as                 Location to save the article page.
slug                    Page slug.
source_path             Full system path of the article source file.
relative_source_path    Relative path from PATH_ to the article source file.
status                  The article status, can be any of 'published' or
                        'draft'.
summary                 Rendered summary content.
tags                    List of :ref:`Tag <object-author_cat_tag>`
                        objects.
template                Template name to use for rendering.
title                   Title of the article.
translations            List of translations
                        :ref:`Article <object-article>` objects.
url                     URL to the article page.
======================  ===================================================

.. _PATH: settings.html#PATH


.. _object-author_cat_tag:

Author / Category / Tag
-----------------------

The string representation of those objects is the `name` attribute.

===================     ===================================================
Attribute               Description
===================     ===================================================
name                    Name of this object [1]_.
page_name               Author page name.
save_as                 Location to save the author page.
slug                    Page slug.
url                     URL to the author page.
===================     ===================================================

.. [1] for Author object, coming from `:authors:` or `AUTHOR`.

.. _object-page:

Page
----

The string representation of a Page is the `source_path` attribute.

=====================  ===================================================
Attribute              Description
=====================  ===================================================
author                 The :ref:`Author <object-author_cat_tag>` of
                       this page.
content                The rendered content of the page.
date                   Datetime object representing the page date.
date_format            Either default date format or locale date format.
default_template       Default template name.
in_default_lang        Boolean representing if the article is written
                       in the default language.
lang                   Language of the article.
locale_date            Date formatted by the `date_format`.
metadata               Page header metadata `dict`.
save_as                Location to save the page.
slug                   Page slug.
source_path            Full system path of the page source file.
relative_source_path   Relative path from PATH_ to the page source file.
status                 The page status, can be any of 'published', 'hidden' or
                       'draft'.
summary                Rendered summary content.
tags                   List of :ref:`Tag <object-author_cat_tag>`
                       objects.
template               Template name to use for rendering.
title                  Title of the page.
translations           List of translations
                       :ref:`Article <object-article>` objects.
url                    URL to the page.
=====================  ===================================================

.. _PATH: settings.html#PATH


Feeds
=====

The feed variables changed in 3.0. Each variable now explicitly lists ATOM or
RSS in the name. ATOM is still the default. Old themes will need to be updated.
Here is a complete list of the feed variables::

    FEED_ATOM
    FEED_RSS
    FEED_ALL_ATOM
    FEED_ALL_RSS
    CATEGORY_FEED_ATOM
    CATEGORY_FEED_RSS
    AUTHOR_FEED_ATOM
    AUTHOR_FEED_RSS
    TAG_FEED_ATOM
    TAG_FEED_RSS
    TRANSLATION_FEED_ATOM
    TRANSLATION_FEED_RSS


Inheritance
===========

Since version 3.0, Pelican supports inheritance from the ``simple`` theme, so
you can re-use the ``simple`` theme templates in your own themes.

If one of the mandatory files in the ``templates/`` directory of your theme is
missing, it will be replaced by the matching template from the ``simple``
theme. So if the HTML structure of a template in the ``simple`` theme is right
for you, you don't have to write a new template from scratch.

You can also extend templates from the ``simple`` theme in your own themes by
using the ``{% extends %}`` directive as in the following example:

.. code-block:: html+jinja

    {% extends "!simple/index.html" %}   <!-- extends the ``index.html`` template from the ``simple`` theme -->

    {% extends "index.html" %}   <!-- "regular" extending -->


Example
-------

With this system, it is possible to create a theme with just two files.

base.html
"""""""""

The first file is the ``templates/base.html`` template:

.. code-block:: html+jinja

    {% extends "!simple/base.html" %}

    {% block head %}
    {{ super() }}
       <link rel="stylesheet" type="text/css" href="{{ SITEURL }}/theme/css/style.css" />
    {% endblock %}

1. On the first line, we extend the ``base.html`` template from the ``simple``
   theme, so we don't have to rewrite the entire file.
2. On the third line, we open the ``head`` block which has already been defined
   in the ``simple`` theme.
3. On the fourth line, the function ``super()`` keeps the content previously
   inserted in the ``head`` block.
4. On the fifth line, we append a stylesheet to the page.
5. On the last line, we close the ``head`` block.

This file will be extended by all the other templates, so the stylesheet will
be linked from all pages.

style.css
"""""""""

The second file is the ``static/css/style.css`` CSS stylesheet:

.. code-block:: css

    body {
        font-family : monospace ;
        font-size : 100% ;
        background-color : white ;
        color : #111 ;
        width : 80% ;
        min-width : 400px ;
        min-height : 200px ;
        padding : 1em ;
        margin : 5% 10% ;
        border : thin solid gray ;
        border-radius : 5px ;
        display : block ;
    }

    a:link    { color : blue ; text-decoration : none ;      }
    a:hover   { color : blue ; text-decoration : underline ; }
    a:visited { color : blue ;                               }

    h1 a { color : inherit !important }
    h2 a { color : inherit !important }
    h3 a { color : inherit !important }
    h4 a { color : inherit !important }
    h5 a { color : inherit !important }
    h6 a { color : inherit !important }

    pre {
        margin : 2em 1em 2em 4em ;
    }

    #menu li {
        display : inline ;
    }

    #post-list {
        margin-bottom : 1em ;
        margin-top : 1em ;
    }

Download
""""""""

You can download this example theme :download:`here <_static/theme-basic.zip>`.
