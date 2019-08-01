编写内容
###############

文章和页面
==================

在Pelican中，"文章" 是按时间顺序的内容，好比博客中的文章，因此它与日期相关联。

而"页面"则是不经常更改的内容（比如 “关于”页面，或者“联系”页面）。

你可以在Pelican官方库的 ``samples/content/`` 找到相关的范例。

.. _internal_metadata:

文件元数据
=============

Pelican 会自动尝试从文件系统中获取需要的信息（例如你的文章的分类类目），
然而有一些信息仍需要你在文件中以元数据的形式提供。

如果你用reStructuredText格式写文章，你可以通过以下语法在文件内提供元数据
（文件扩展名为 ``.rst`` ）::

    标题标题标题
    ##############

    :date: 2010-10-03 10:20
    :modified: 2010-10-04 18:40
    :tags: thats, awesome
    :category: yeah
    :slug: my-super-post
    :authors: Alexis Metaireau, Conan Doyle
    :summary: 摘要摘要，供索引及feeds用

其中的作者列表Authors和标签列表tags，可以用分号当分隔符，这样单个作者名中或tag中可以包含逗号::

    :tags: pelican, publishing tool; pelican, bird
    :authors: Metaireau, Alexis; Doyle, Conan

Pelican实现扩展支持reStructuredText文件中使用 ``abbr`` HTML标签. 如果要使用的话，在文件中类似下面这样写::

    这里的内容会被编译 :abbr:`HTML (HyperText Markup Language)`.

你还可以用Markdown编写（扩展名为 ``.md``, ``.markdown``, ``.mkd`` 或者 ``.mdown`` 的文件）。
不过需要确保安装了 ``Markdown`` 模块（通过命令 ``pip install Markdown`` 安装）。

Pelican也支持Markdown的扩展 `Markdown Extensions`_ ，如果这些扩展没有被包含在 ``Markdown`` 模块中的话，
则需要单独安装， 安装后可以在  ``Markdown`` 设置中加载。


Markdown中写元数据的语法如下::

    Title: 标题标题
    Date: 2010-12-03 10:20
    Modified: 2010-12-05 19:30
    Category: Python
    Tags: pelican, publishing
    Slug: my-super-post
    Authors: Alexis Metaireau, Conan Doyle
    Summary: 摘要摘要

    正文内容内容

你还可以在python模板中使用自己的元数据关键字（只要它们不与保留的元数据关键字冲突）。 以下是元数据保留关键字列表:

* `Title`
* `Tags`
* `Date`
* `Modified`
* `Status`
* `Category`
* `Author`
* `Authors`
* `Slug`
* `Summary`
* `Template`
* `Save_as`
* `Url`

其他文件格式（如 AsciiDoc_ ）可通过插件获得。 请参阅Pelican官方库中 `pelican-plugins`_ 部分。

Pelican还可以处理以``.html``和``.htm``结尾的HTML文件。 Pelican以非常简单的方式解释HTML，
从 ``meta`` 标签读取元数据，从 ``title`` 标签读取标题，从 ``body`` 标签读取正文::

    <html>
        <head>
            <title>My super title</title>
            <meta name="tags" content="thats, awesome" />
            <meta name="date" content="2012-07-09 22:28" />
            <meta name="modified" content="2012-07-10 20:14" />
            <meta name="category" content="yeah" />
            <meta name="authors" content="Alexis Métaireau, Conan Doyle" />
            <meta name="summary" content="Short version for index and feeds" />
        </head>
        <body>
            This is the content of my super blog post.
        </body>
    </html>

HTML的标准元数据中有一个要注意的地方：标签tag在Pelican标准中是通过 ``tags`` 元数据指定的；
而在HTML标准中则通过 ``keywords`` 元数据指定。这两者可以互换使用。

请注意，除标题必需外，其他元数据都不是必需的：
如果没有指定日期并且 ``DEFAULT_DATE`` 设置为 ``'fs'`` 的话，Pelican将根据文件的 “mtime” 时间戳生成日期；
而分类则会根据文件所在的目录决定。例如，位于 ``python/foobar/myfoobar.rst`` 的文件将被归类为 ``foobar`` 。
如果你不想Pelican用子目录分类，而想以其他方式组织文件，你可以设置 ``USE_FOLDER_AS_CATEGORY`` 设置为 ``False`` 。
在解析日期元数据时，Pelican遵循W3C标准  `suggested subset ISO 8601`__ 。

所以标题是唯一需要的元数据。如果这个也让你不爽，不用担心。
你可以使用文章的文件名作为标题，而不是每次在文件内的元数据部分手动指定。例如，一篇Markdown文章的文件名为 ``Publishing via Pelican.md`` ，
系统自动提取出 *Publishing via Pelican* 作为文章的标题title。
你可以将下面这条语句添加到设置文件中以启用这个功能::

    FILENAME_METADATA = '(?P<title>.*)'

.. note::

   在尝试不同的设置（尤其是元数据设置）时，有可能会受缓存干扰，作的修改可能不见生效。 
   在这种情况下，可以使用 ``LOAD_CONTENT_CACHE = False`` 禁用缓存或
   使用 ``--ignore-cache`` 命令行开关。

__ `W3C ISO 8601`_

``modified`` 记录的应该是你对文章最后的一次更新，如果没有其它指定默认为 ``date`` 。
此外，你可以在模板中显示 ``modified`` ，当你修改文章后将 ``modified`` 设置为当前日期时，
Feed阅读器中的Feed条目将自动更新。

``authors`` 是以逗号分隔的文章作者列表。如果只有一个作者，你可以使用 ``author`` 字段。

如果不想在文件中写摘要提供summary元数据，则可以使用 ``SUMMARY_MAX_LENGTH`` 来设置从文章起始读取多少个字数作
为文章的摘要。

在设置 ``FILENAME_METADATA`` 属性这里，你可以用正则表达式从文件名中提取出元数据。
匹配的所有命名组都将设置在元数据对象中。 ``FILENAME_METADATA`` 设置的默认值是仅从文件名中提取日期。
例如，如果你想提取日期date和slug，你可以这样写： ``'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'``

请注意，文件中可用的元数据优先于从文件名中提取的元数据。

页面
=====

如果在content目录中创建 `pages` 文件夹，所有pages目录中的文件将被生成静态页面，
例如 **关于** 页面或 **联系人** 页面。 （请参阅下面的示例文件系统的布局。）

您可以设置 ``DISPLAY_PAGES_ON_MENU`` 来控制是否在主导航菜单中显示这些页面。（默认为 ``True`` 。）

如果你想要某些页面不要在导航菜单中显示或者不要被指向，可以在页面文件的元数据中添加 ``status: hidden`` 属性。
比如制作一个配合主题风格的404静态页面时，就可以给这个页面作这样的设置。

静态内容
==============

静态文件是除了文章和页面之外的文件，这些文件不需要处理，会直接被复制到output输出文件夹。
您可以在项目配置文件 ``pelicanconf.py`` 中的 ``STATIC_PATHS`` 指定静态文件。 
Pelican默认设置了 ``images`` 为静态文件目录，其他目录则需要你手动添加。
此外，还包括明确链接到的静态文件（见下文）。

同一目录中的混合内容
-----------------------------------

从Pelican 3.5开始，静态文件可以安全地与页面源文件共享同一目录，而不会在生成的站点中公开页面源文件。
这种目录必须同时被添加到 ``STATIC_PATHS`` 和 ``PAGE_PATHS`` 
（或者 ``STATIC_PATHS`` 和 ``ARTICLE_PATHS`` ）中。
Pelican将正常识别和处理页面源文件并复制其余文件，就像将其余文件视为是在单独目录中的静态文件一样。

注意：将静态文件和内容源文件放在同一源目录中并不能保证它们最终会在生成的站点中的同一位置。
最简单的方法是使用 ``{attach}`` 链接语法（如下所述）。又或者，
可以设置 ``STATIC_SAVE_AS`` ， ``PAGE_SAVE_AS`` 和 ``ARTICLE_SAVE_AS`` 这三个属性
（以及相应的 ``* _URL`` 属性）将不同类型的文件放在一起，就像早期版本的Pelican一样。

.. _ref-linking-to-internal-content:

链接到内部内容
===========================

从Pelican 3.1开始，可以在 *源内容* 层次结构中指定文件的站点内链接，而不用在 *生成内容* 层次结构中指定。 
这样可以更容易地从当前文章链接到可能与该文章并列的其他内容（而不必去确定每次生成站点时那些"其他内容"会被放在何处）。



.. _W3C ISO 8601: https://www.w3.org/TR/NOTE-datetime
.. _AsciiDoc: http://www.methods.co.nz/asciidoc/
.. _pelican-plugins: https://github.com/getpelican/pelican-plugins
.. _Markdown Extensions: https://python-markdown.github.io/extensions/
