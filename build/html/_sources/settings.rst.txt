设置
########

可以通过命令行给Pelican指定一份设置文件，用以配置Pelican::

    pelican content -s path/to/your/pelicanconf.py

如果你初始化用的是 ``pelican-quickstart`` 命令, 你的主配置文件默认名将为 ``pelicanconf.py``

.. note::

   在尝试不同的设置（尤其是设置元数据）时，有可能会受缓存干扰，作的修改可能不见生效。 
   这种情况下，可以使用 ``LOAD_CONTENT_CACHE = False`` 禁用缓存或
   使用 ``--ignore-cache`` 命令行开关。 

设置文件是以Python模块（一个文件）的方式进行配置，这里有一份 
`示例设置文件
<https://github.com/getpelican/pelican/raw/master/samples/pelican.conf.py>`_ 
供参考

要查看环境中的当前设置列表（包括默认值和任何自定义值），
请运行以下命令（将一个或多个特定的设置关键字名称附加为参数以查看这些设置的值）::

    pelican --print-settings

所有设置的标识符必须全部大写，否则不会被处理。
设置数字（5,20等），布尔值（True，False，None等），
字典或元组的值 *不* 应该用引号括起来。所有其他值（即字符串） *必须* 用引号括起来。

除非另有说明，否则引用路径的设置可以是绝对路径，也可以是相对于配置文件的相对路径。
在配置文件中定义的设置将传递给模板，进而应用于设置站点范围的内容。

以下为Pelican设置项列表:


基本设置
==============

.. data:: USE_FOLDER_AS_CATEGORY = True

   如果不想在文章中的元数据中指定分类，请将此设置设为 ``True``，并用子目录来放置组织
   你的文章，如此这些子目录将成为你的网站文章的分类。如果设置值 为 ``False``，则会根
   据 ``DEFAULT_CATEGORY`` 中的值来设置。

.. data:: DEFAULT_CATEGORY = 'misc'

   默认的分类属性（备用用）

.. data:: DISPLAY_PAGES_ON_MENU = True

   是否在模板菜单上显示页面。 模板可能会也可能不会遵循此设置。

.. data:: DISPLAY_CATEGORIES_ON_MENU = True

   是否在模板菜单上显示分类。 模板可能会也可能不会遵循此设置。

.. data:: DOCUTILS_SETTINGS = {}

   设置docutils发布者的额外配置（仅适用于reStructuredText）。 有关详细信息，请参
   阅 `Docutils Configuration`_ 。 

.. data:: DELETE_OUTPUT_DIRECTORY = False

   在生成新站点之前，删除文件输出output目录以及其中 **所有** 内容, 这可以用于防止
   旧的，不必要的文件长期保留在output目录中。但是， **这是一种破坏性的设置，应该特
   别小心对待。**

.. data:: OUTPUT_RETENTION = []

   值为一个文件名列表，表示在output目录中的这些/这类文件将被保留不会删除。
   其中一个用法是保存版本控制数据。

   示例::

      OUTPUT_RETENTION = [".hg", ".git", ".bzr"]

.. data:: JINJA_ENVIRONMENT = {'trim_blocks': True, 'lstrip_blocks': True}

   给使用的Jinja2自定义环境变量，还可以包含使用的扩展列表，
   详情参照 `Jinja Environment documentation`_

.. data:: JINJA_FILTERS = {}

   给使用的Jinja2自定义过滤器，字典类型。
   数据为过滤器名称对应过滤器函数。

   示例::

    JINJA_FILTERS = {'urlencode': urlencode_filter}

   参考 `Jinja custom filters documentation`_

.. data:: LOG_FILTER = []

   包含日志记录级别（最高为"warning"）和要忽略的消息的元组列表。

   示例::

      LOG_FILTER = [(logging.WARN, 'TAG_SAVE_AS is set to False')]

.. data:: READERS = {}

   Pelican要处理或忽略的文件扩展名或Reader类。

   例如, 为了避免处理.html文件，设置::

      READERS = {'html': None}

   为 ``foo`` 扩展添加自定义阅读器，设置::

      READERS = {'foo': FooReader}

.. data:: IGNORE_FILES = ['.#*']

   一个全局模式列表。 处理器将忽略这里匹配出的文件和目录。
   例如，默认值 ``['.#*']`` 表示将忽略emacs锁定文件，
   而 ``['__pycache__']`` 表示忽略Python 3的缓存文件。

.. data:: MARKDOWN = {...}

   Markdown处理器的额外配置。 有关支持的选项的完整列表，请参阅Python Markdown文档
   的 `选项部分 <https://python-markdown.github.io/reference/#markdown>`_ 。
   ``extensions`` 选项将从 ``extension_configs`` 选项中自动计算出来

   默认为::

        MARKDOWN = {
            'extension_configs': {
                'markdown.extensions.codehilite': {'css_class': 'highlight'},
                'markdown.extensions.extra': {},
                'markdown.extensions.meta': {},
            },
            'output_format': 'html5',
        }

   .. Note::
      在设置文件中赋值会覆盖这里的默认值。

.. data:: OUTPUT_PATH = 'output/'

   定义生成的文件的保存路径

.. data:: PATH

   Pelican要处理的内容目录的路径。
   如果未定义，并且没有通过 ``pelican`` 命令的参数指定路径，
   Pelican将使用当前的工作目录。

.. data:: PAGE_PATHS = ['pages']

   '页面'的目录和文件列表，为相对于 ``PATH`` 的相对路径。

.. data:: PAGE_EXCLUDES = []

   除了 ``ARTICLE_PATHS`` 之外，在查找'页面'时要排除的目录列表。

.. data:: ARTICLE_PATHS = ['']

   '文章'的目录和文件列表, 为相对于 ``PATH`` 的相对路径。

.. data:: ARTICLE_EXCLUDES = []

   除了 ``PAGE_PATHS`` 之外，在查找'文章'时要排除的目录列表。

.. data:: OUTPUT_SOURCES = False

   如果要将文章和页面以其原始格式（例如Markdown或reStructuredText）复制到指
   定的  ``OUTPUT_PATH`` ，则设置为True。

.. data:: OUTPUT_SOURCES_EXTENSION = '.text'

   控制SourcesGenerator将使用的扩展名。 默认为 ``.text`` 。 
   如果不是有效字符串，则将使用默认值。

.. data:: PLUGINS = []

   加载的插件列表。参考 :ref:`plugins`.

.. data:: PLUGIN_PATHS = []

   查找插件的目录列表. 参考 :ref:`plugins`.

.. data:: SITENAME = 'A Pelican Blog'

   你的站点名称

.. data:: SITEURL

   你的网站的基本网址。默认情况下为未定义，因此最好指定你的网站URL；如果不指定，
   则不能使用格式正确的URL生成订阅源。如果你的网站可以通过HTTPS访问，这里的值
   应该以 ``https://`` 开头，否则以 ``http://`` 开头。 然后加上你的域名，最后没有斜杠。
   示例: ``SITEURL = 'https://example.com'``

.. data:: STATIC_PATHS = ['images']

   一个用于存放静态文件的目录列表 (相对于 ``PATH`` ) 。这些文件会直接被复制到output
   目录而不作修改，通常会跳过文章，页面和其他内容源文件，所以同一个目录出现在这里和
   ``PAGE_PATHS`` 或 ``ARTICLE_PATHS`` 是没问题的。默认值为"images" 目录

.. data:: STATIC_EXCLUDES = []

   在查找静态文件时要排除的目录列表。

.. data:: STATIC_EXCLUDE_SOURCES = True

   如果设置为False，则在复制 ``STATIC_PATHS`` 中找到的文件时不会跳过内容源文件。
   此设置用于向后兼容3.5版之前的Pelican版本。
   除非 ``STATIC_PATHS`` 包含一个同样位于 ``ARTICLE_PATHS`` 或 ``PAGE_PATHS`` 的
   目录，不然此设置不会生效。如果您尝试发布站点的源文件，
   请考虑使用 ``OUTPUT_SOURCES`` 设置。

.. data:: STATIC_CREATE_LINKS = False

   为静态文件创建链接而不是复制文件。如果content目录和output目录位于同一设备上，
   则创建硬链接。 如果这两个目录在不同的文件系统上，用符号链接。如果创建了符号链接，
   记得在上传站点时向rsync添加 ``-L`` 或 ``--copy-links`` 选项。

.. data:: STATIC_CHECK_IF_MODIFIED = False

   如果设置为 ``True`` ，并且 ``STATIC_CREATE_LINKS`` 为 ``False`` ，
   则比较内容和输出文件的mtimes时间戳，只复制比现有输出文件更新的内容文件。

.. data:: TYPOGRIFY = False

   如果设置为True, 则通过 `Typogrify <https://pypi.python.org/pypi/typogrify>`_ 库
   将几个排版改进合并到生成的HTML中，
   Typogrify库可以用这句命令安装: ``pip install typogrify`` 。

.. data:: TYPOGRIFY_IGNORE_TAGS = []

   要忽略的Typogrify标记列表。默认情况下，Typogrify将忽略 ``pre`` 和 ``code`` 标签。 
   这需要安装Typogrify版本2.0.4或更高版本

.. data:: SUMMARY_MAX_LENGTH = 50

   创建文章摘要时的字数，默认50（以单词测量）。这仅适用于您的内容未另外指定摘要的情况。
   设置为 ``None`` 的话将使摘要成为原始内容的副本。
   (measured in words) of the text created.  This only applies if your content
   does not otherwise specify a summary. Setting to ``None`` will cause the
   summary to be a copy of the original content.

.. data:: WITH_FUTURE_DATES = True

   如果禁用，则在文件中指定日期为未来的日期时会使文件默认状态为草稿 ``draft`` 。
   参考 :ref:`reading_only_modified_content` 

.. data:: INTRASITE_LINK_REGEX = '[{|](?P<what>.*?)[|}]'

   用于解析内部链接的正则表达式。链接到内部文件，标签等时的默认语法是
   在 ``{}`` 或 ``||`` 中包含标识符，比如 ``filename`` 。 ``{`` 和 ``}`` 之
   间的标识符进入 ``what`` 捕获组。详情见 :ref:`ref-linking-to-internal-content`.

.. data:: PYGMENTS_RST_OPTIONS = []

   reStructuredText代码块的默认Pygments（语法高亮）设置列表。
   请参阅 :ref:`internal_pygments_options` 以获取支持的选项列表。

.. data:: SLUGIFY_SOURCE = 'title'

   指定从中里自动生成slug内容。 可以设置为 ``title`` 以使用'Title：'元数据标签
   或者使用 ``basename`` 以使用文章的文件名来创建slug。

.. data:: CACHE_CONTENT = False

   如果设置 ``True``, 则将内容保存在缓存中。有关缓存的详细信息，
   参考 :ref:`reading_only_modified_content` 。

.. data:: CONTENT_CACHING_LAYER = 'reader'

   如果设为 ``'reader'`` , 则仅保存阅读器返回的原始内容和元数据。
   如果设为 ``'generator'``, 则保存已处理的内容对象。

.. data:: CACHE_PATH = 'cache'

   用于存储缓存文件的目录。

.. data:: GZIP_CACHE = True

   如果设为 ``True``, 使用gzip来压缩/解压缩缓存文件。

.. data:: CHECK_MODIFIED_METHOD = 'mtime'

   Controls how files are checked for modifications.

.. data:: LOAD_CONTENT_CACHE = False

   If ``True``, load unmodified content from caches.

.. data:: WRITE_SELECTED = []

   If this list is not empty, **only** output files with their paths in this
   list are written. Paths should be either absolute or relative to the current
   Pelican working directory. For possible use cases see
   :ref:`writing_only_selected_content`.

.. data:: FORMATTED_FIELDS = ['summary']

   A list of metadata fields containing reST/Markdown content to be parsed and
   translated to HTML.

.. data:: PORT = 8000

   The TCP port to serve content from the output folder via HTTP when pelican
   is run with --listen

.. data:: BIND = ''

   The IP to which to bind the HTTP server.


URL settings
============

The first thing to understand is that there are currently two supported methods
for URL formation: *relative* and *absolute*. Relative URLs are useful when
testing locally, and absolute URLs are reliable and most useful when
publishing. One method of supporting both is to have one Pelican configuration
file for local development and another for publishing. To see an example of
this type of setup, use the ``pelican-quickstart`` script as described in the
:doc:`Installation <install>` section, which will produce two separate
configuration files for local development and publishing, respectively.

You can customize the URLs and locations where files will be saved. The
``*_URL`` and ``*_SAVE_AS`` variables use Python's format strings. These
variables allow you to place your articles in a location such as
``{slug}/index.html`` and link to them as ``{slug}`` for clean URLs (see
example below). These settings give you the flexibility to place your articles
and pages anywhere you want.

.. note::
    If you specify a ``datetime`` directive, it will be substituted using the
    input files' date metadata attribute. If the date is not specified for a
    particular file, Pelican will rely on the file's ``mtime`` timestamp. Check
    the `Python datetime documentation`_ for more information.

.. _Python datetime documentation:
    https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior

Also, you can use other file metadata attributes as well:

* slug
* date
* lang
* author
* category

Example usage::

   ARTICLE_URL = 'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/'
   ARTICLE_SAVE_AS = 'posts/{date:%Y}/{date:%b}/{date:%d}/{slug}/index.html'
   PAGE_URL = 'pages/{slug}/'
   PAGE_SAVE_AS = 'pages/{slug}/index.html'

This would save your articles into something like
``/posts/2011/Aug/07/sample-post/index.html``, save your pages into
``/pages/about/index.html``, and render them available at URLs of
``/posts/2011/Aug/07/sample-post/`` and ``/pages/about/``, respectively.

.. data:: RELATIVE_URLS = False

   Defines whether Pelican should use document-relative URLs or not. Only set
   this to ``True`` when developing/testing and only if you fully understand
   the effect it can have on links/feeds.

.. data:: ARTICLE_URL = '{slug}.html'

   The URL to refer to an article.

.. data:: ARTICLE_SAVE_AS = '{slug}.html'

   The place where we will save an article.

.. data:: ARTICLE_LANG_URL = '{slug}-{lang}.html'

   The URL to refer to an article which doesn't use the default language.

.. data:: ARTICLE_LANG_SAVE_AS = '{slug}-{lang}.html'

   The place where we will save an article which doesn't use the default
   language.

.. data:: DRAFT_URL = 'drafts/{slug}.html'

   The URL to refer to an article draft.

.. data:: DRAFT_SAVE_AS = 'drafts/{slug}.html'

   The place where we will save an article draft.

.. data:: DRAFT_LANG_URL = 'drafts/{slug}-{lang}.html'

   The URL to refer to an article draft which doesn't use the default language.

.. data:: DRAFT_LANG_SAVE_AS = 'drafts/{slug}-{lang}.html'

   The place where we will save an article draft which doesn't use the default
   language.

.. data:: PAGE_URL = 'pages/{slug}.html'

   The URL we will use to link to a page.

.. data:: PAGE_SAVE_AS = 'pages/{slug}.html'

   The location we will save the page. This value has to be the same as
   PAGE_URL or you need to use a rewrite in your server config.

.. data:: PAGE_LANG_URL = 'pages/{slug}-{lang}.html'

   The URL we will use to link to a page which doesn't use the default
   language.

.. data:: PAGE_LANG_SAVE_AS = 'pages/{slug}-{lang}.html'

   The location we will save the page which doesn't use the default language.

.. data:: DRAFT_PAGE_URL = 'drafts/pages/{slug}.html'

   The URL used to link to a page draft.

.. data:: DRAFT_PAGE_SAVE_AS = 'drafts/pages/{slug}.html'

   The actual location a page draft is saved at.

.. data:: DRAFT_PAGE_LANG_URL = 'drafts/pages/{slug}-{lang}.html'

   The URL used to link to a page draft which doesn't use the default
   language.

.. data:: DRAFT_PAGE_LANG_SAVE_AS = 'drafts/pages/{slug}-{lang}.html'

   The actual location a page draft which doesn't use the default language is
   saved at.

.. data:: AUTHOR_URL = 'author/{slug}.html'

   The URL to use for an author.

.. data:: AUTHOR_SAVE_AS = 'author/{slug}.html'

   The location to save an author.

.. data:: CATEGORY_URL = 'category/{slug}.html'

   The URL to use for a category.

.. data:: CATEGORY_SAVE_AS = 'category/{slug}.html'

   The location to save a category.

.. data:: TAG_URL = 'tag/{slug}.html'

   The URL to use for a tag.

.. data:: TAG_SAVE_AS = 'tag/{slug}.html'

   The location to save the tag page.

.. note::

    If you do not want one or more of the default pages to be created (e.g.,
    you are the only author on your site and thus do not need an Authors page),
    set the corresponding ``*_SAVE_AS`` setting to ``''`` to prevent the
    relevant page from being generated.

Pelican can optionally create per-year, per-month, and per-day archives of your
posts. These secondary archives are disabled by default but are automatically
enabled if you supply format strings for their respective ``_SAVE_AS``
settings. Period archives fit intuitively with the hierarchical model of web
URLs and can make it easier for readers to navigate through the posts you've
written over time.

Example usage::

   YEAR_ARCHIVE_SAVE_AS = 'posts/{date:%Y}/index.html'
   MONTH_ARCHIVE_SAVE_AS = 'posts/{date:%Y}/{date:%b}/index.html'

With these settings, Pelican will create an archive of all your posts for the
year at (for instance) ``posts/2011/index.html`` and an archive of all your
posts for the month at ``posts/2011/Aug/index.html``.

.. note::
    Period archives work best when the final path segment is ``index.html``.
    This way a reader can remove a portion of your URL and automatically arrive
    at an appropriate archive of posts, without having to specify a page name.

.. data:: YEAR_ARCHIVE_URL = ''

   The URL to use for per-year archives of your posts. Used only if you have
   the ``{url}`` placeholder in ``PAGINATION_PATTERNS``.

.. data:: YEAR_ARCHIVE_SAVE_AS = ''

   The location to save per-year archives of your posts.

.. data:: MONTH_ARCHIVE_URL = ''

   The URL to use for per-month archives of your posts. Used only if you have
   the ``{url}`` placeholder in ``PAGINATION_PATTERNS``.

.. data:: MONTH_ARCHIVE_SAVE_AS = ''

   The location to save per-month archives of your posts.

.. data:: DAY_ARCHIVE_URL = ''

   The URL to use for per-day archives of your posts. Used only if you have the
   ``{url}`` placeholder in ``PAGINATION_PATTERNS``.

.. data:: DAY_ARCHIVE_SAVE_AS = ''

   The location to save per-day archives of your posts.

``DIRECT_TEMPLATES`` work a bit differently than noted above. Only the
``_SAVE_AS`` settings are available, but it is available for any direct
template.

.. data:: ARCHIVES_SAVE_AS = 'archives.html'

   The location to save the article archives page.

.. data:: AUTHORS_SAVE_AS = 'authors.html'

   The location to save the author list.

.. data:: CATEGORIES_SAVE_AS = 'categories.html'

   The location to save the category list.

.. data:: TAGS_SAVE_AS = 'tags.html'

   The location to save the tag list.

.. data:: INDEX_SAVE_AS = 'index.html'

   The location to save the list of all articles.

URLs for direct template pages are theme-dependent. Some themes use
corresponding ``*_URL`` setting as string, while others hard-code them:
``'archives.html'``, ``'authors.html'``, ``'categories.html'``,
``'tags.html'``.

.. data:: SLUG_REGEX_SUBSTITUTIONS = [
        (r'[^\\w\\s-]', ''),  # remove non-alphabetical/whitespace/'-' chars
        (r'(?u)\\A\\s*', ''),  # strip leading whitespace
        (r'(?u)\\s*\\Z', ''),  # strip trailing whitespace
        (r'[-\\s]+', '-'),  # reduce multiple whitespace or '-' to single '-'
    ]

   Regex substitutions to make when generating slugs of articles and pages.
   Specified as a list of pairs of ``(from, to)`` which are applied in order,
   ignoring case. The default substitutions have the effect of removing
   non-alphanumeric characters and converting internal whitespace to dashes.
   Apart from these substitutions, slugs are always converted to lowercase
   ascii characters and leading and trailing whitespace is stripped. Useful for
   backward compatibility with existing URLs.

.. data:: AUTHOR_REGEX_SUBSTITUTIONS = SLUG_REGEX_SUBSTITUTIONS

   Regex substitutions for author slugs. Defaults to
   ``SLUG_REGEX_SUBSTITUTIONS``.

.. data:: CATEGORY_REGEX_SUBSTITUTIONS = SLUG_REGEX_SUBSTITUTIONS

   Regex substitutions for category slugs. Defaults to
   ``SLUG_REGEX_SUBSTITUTIONS``.

.. data:: TAG_REGEX_SUBSTITUTIONS = SLUG_REGEX_SUBSTITUTIONS

   Regex substitutions for tag slugs. Defaults to ``SLUG_REGEX_SUBSTITUTIONS``.

Time and Date
=============

.. data:: TIMEZONE

   The timezone used in the date information, to generate Atom and RSS feeds.

   If no timezone is defined, UTC is assumed. This means that the generated
   Atom and RSS feeds will contain incorrect date information if your locale is
   not UTC.

   Pelican issues a warning in case this setting is not defined, as it was not
   mandatory in previous versions.

   Have a look at `the wikipedia page`_ to get a list of valid timezone values.

.. _the wikipedia page: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

.. data:: DEFAULT_DATE = None

   The default date you want to use.  If ``'fs'``, Pelican will use the file
   system timestamp information (mtime) if it can't get date information from
   the metadata.  If given any other string, it will be parsed by the same
   method as article metadata.  If set to a tuple object, the default datetime
   object will instead be generated by passing the tuple to the
   ``datetime.datetime`` constructor.

.. data:: DEFAULT_DATE_FORMAT = '%a %d %B %Y'

   The default date format you want to use.

.. data:: DATE_FORMATS = {}

   If you manage multiple languages, you can set the date formatting here.

   If no ``DATE_FORMATS`` are set, Pelican will fall back to
   ``DEFAULT_DATE_FORMAT``. If you need to maintain multiple languages with
   different date formats, you can set the ``DATE_FORMATS`` dictionary using
   the language name (``lang`` metadata in your post content) as the key.

   In addition to the standard C89 strftime format codes that are listed in
   `Python strftime documentation`_, you can use the ``-`` character between
   ``%`` and the format character to remove any leading zeros. For example,
   ``%d/%m/%Y`` will output ``01/01/2014`` whereas ``%-d/%-m/%Y`` will result
   in ``1/1/2014``.

   .. parsed-literal::

       DATE_FORMATS = {
           'en': '%a, %d %b %Y',
           'jp': '%Y-%m-%d(%a)',
       }

   It is also possible to set different locale settings for each language by
   using a ``(locale, format)`` tuple as a dictionary value which will override
   the ``LOCALE`` setting:

   .. parsed-literal::

      # On Unix/Linux
      DATE_FORMATS = {
          'en': ('en_US','%a, %d %b %Y'),
          'jp': ('ja_JP','%Y-%m-%d(%a)'),
      }

      # On Windows
      DATE_FORMATS = {
          'en': ('usa','%a, %d %b %Y'),
          'jp': ('jpn','%Y-%m-%d(%a)'),
      }

.. data:: LOCALE

   Change the locale [#]_. A list of locales can be provided here or a single
   string representing one locale.  When providing a list, all the locales will
   be tried until one works.

   You can set locale to further control date format:

   .. parsed-literal::

       LOCALE = ('usa', 'jpn',      # On Windows
                 'en_US', 'ja_JP'   # On Unix/Linux
      )

   For a list of available locales refer to `locales on Windows`_  or on
   Unix/Linux, use the ``locale -a`` command; see manpage
   `locale(1)`_ for more information.


.. [#] Default is the system locale.

.. _Python strftime documentation: https://docs.python.org/library/datetime.html#strftime-strptime-behavior

.. _locales on Windows: http://msdn.microsoft.com/en-us/library/cdax410z%28VS.71%29.aspx

.. _locale(1): https://linux.die.net/man/1/locale


.. _template_pages:

Template pages
==============

.. data:: TEMPLATE_PAGES = None

   A mapping containing template pages that will be rendered with the blog
   entries. See :ref:`template_pages`.

   If you want to generate custom pages besides your blog entries, you can
   point any Jinja2 template file with a path pointing to the file and the
   destination path for the generated file.

   For instance, if you have a blog with three static pages — a list of books,
   your resume, and a contact page — you could have::

       TEMPLATE_PAGES = {'src/books.html': 'dest/books.html',
                         'src/resume.html': 'dest/resume.html',
                         'src/contact.html': 'dest/contact.html'}

.. data:: TEMPLATE_EXTENSIONS = ['.html']

   The extensions to use when looking up template files from template names.

.. data:: DIRECT_TEMPLATES = ['index', 'authors', 'categories', 'tags', 'archives']

   List of templates that are used directly to render content. Typically direct
   templates are used to generate index pages for collections of content (e.g.,
   category and tag index pages). If the author, category and tag collections are not
   needed, set ``DIRECT_TEMPLATES = ['index', 'archives']``

   ``DIRECT_TEMPLATES`` are searched for over paths maintained in
   ``THEME_TEMPLATES_OVERRIDES``.


Metadata
========

.. data:: AUTHOR

   Default author (usually your name).

.. data:: DEFAULT_METADATA = {}

   The default metadata you want to use for all articles and pages.

.. data:: FILENAME_METADATA = r'(?P<date>\d{4}-\d{2}-\d{2}).*'

   The regexp that will be used to extract any metadata from the filename. All
   named groups that are matched will be set in the metadata object.  The
   default value will only extract the date from the filename.

   For example, to extract both the date and the slug::

      FILENAME_METADATA = r'(?P<date>\d{4}-\d{2}-\d{2})_(?P<slug>.*)'

   See also ``SLUGIFY_SOURCE``.

.. data:: PATH_METADATA = ''

   Like ``FILENAME_METADATA``, but parsed from a page's full path relative to
   the content source directory.

.. data:: EXTRA_PATH_METADATA = {}

   Extra metadata dictionaries keyed by relative path. Relative paths require
   correct OS-specific directory separators (i.e. / in UNIX and \\ in Windows)
   unlike some other Pelican file settings. Paths to a directory apply to all
   files under it. The most-specific path wins conflicts.

Not all metadata needs to be :ref:`embedded in source file itself
<internal_metadata>`. For example, blog posts are often named following a
``YYYY-MM-DD-SLUG.rst`` pattern, or nested into ``YYYY/MM/DD-SLUG``
directories. To extract metadata from the filename or path, set
``FILENAME_METADATA`` or ``PATH_METADATA`` to regular expressions that use
Python's `group name notation`_ ``(?P<name>…)``. If you want to attach
additional metadata but don't want to encode it in the path, you can set
``EXTRA_PATH_METADATA``:

.. parsed-literal::

    EXTRA_PATH_METADATA = {
        'relative/path/to/file-1': {
            'key-1a': 'value-1a',
            'key-1b': 'value-1b',
            },
        'relative/path/to/file-2': {
            'key-2': 'value-2',
            },
        }

This can be a convenient way to shift the installed location of a particular
file:

.. parsed-literal::

    # Take advantage of the following defaults
    # STATIC_SAVE_AS = '{path}'
    # STATIC_URL = '{path}'
    STATIC_PATHS = [
        'static/robots.txt',
        ]
    EXTRA_PATH_METADATA = {
        'static/robots.txt': {'path': 'robots.txt'},
        }

.. _group name notation:
   https://docs.python.org/3/library/re.html#regular-expression-syntax


Feed settings
=============

By default, Pelican uses Atom feeds. However, it is also possible to use RSS
feeds if you prefer.

Pelican generates category feeds as well as feeds for all your articles. It
does not generate feeds for tags by default, but it is possible to do so using
the ``TAG_FEED_ATOM`` and ``TAG_FEED_RSS`` settings:

.. data:: FEED_DOMAIN = None, i.e. base URL is "/"

   The domain prepended to feed URLs. Since feed URLs should always be
   absolute, it is highly recommended to define this (e.g.,
   "https://feeds.example.com"). If you have already explicitly defined SITEURL
   (see above) and want to use the same domain for your feeds, you can just
   set:  ``FEED_DOMAIN = SITEURL``.

.. data:: FEED_ATOM = None, i.e. no Atom feed

   The location to save the Atom feed.

.. data:: FEED_ATOM_URL = None

   Relative URL of the Atom feed. If not set, ``FEED_ATOM`` is used both for
   save location and URL.

.. data:: FEED_RSS = None, i.e. no RSS

   The location to save the RSS feed.

.. data:: FEED_RSS_URL = None

   Relative URL of the RSS feed. If not set, ``FEED_RSS`` is used both for save
   location and URL.

.. data:: FEED_ALL_ATOM = 'feeds/all.atom.xml'

   The location to save the all-posts Atom feed: this feed will contain all
   posts regardless of their language.

.. data:: FEED_ALL_ATOM_URL = None

   Relative URL of the all-posts Atom feed. If not set, ``FEED_ALL_ATOM`` is
   used both for save location and URL.

.. data:: FEED_ALL_RSS = None, i.e. no all-posts RSS

   The location to save the the all-posts RSS feed: this feed will contain all
   posts regardless of their language.

.. data:: FEED_ALL_RSS_URL = None

   Relative URL of the all-posts RSS feed. If not set, ``FEED_ALL_RSS`` is used
   both for save location and URL.

.. data:: CATEGORY_FEED_ATOM = 'feeds/{slug}.atom.xml'

   The location to save the category Atom feeds. [2]_

.. data:: CATEGORY_FEED_ATOM_URL = None

   Relative URL of the category Atom feeds, including the ``{slug}``
   placeholder. [2]_ If not set, ``CATEGORY_FEED_ATOM`` is used both for save
   location and URL.

.. data:: CATEGORY_FEED_RSS = None, i.e. no RSS

   The location to save the category RSS feeds, including the ``{slug}``
   placeholder. [2]_

.. data:: CATEGORY_FEED_RSS_URL = None

   Relative URL of the category RSS feeds, including the ``{slug}``
   placeholder. [2]_ If not set, ``CATEGORY_FEED_RSS`` is used both for save
   location and URL.

.. data:: AUTHOR_FEED_ATOM = 'feeds/{slug}.atom.xml'

   The location to save the author Atom feeds. [2]_

.. data:: AUTHOR_FEED_ATOM_URL = None

   Relative URL of the author Atom feeds, including the ``{slug}`` placeholder.
   [2]_ If not set, ``AUTHOR_FEED_ATOM`` is used both for save location and
   URL.

.. data:: AUTHOR_FEED_RSS = 'feeds/{slug}.rss.xml'

   The location to save the author RSS feeds. [2]_

.. data:: AUTHOR_FEED_RSS_URL = None

   Relative URL of the author RSS feeds, including the ``{slug}`` placeholder.
   [2]_ If not set, ``AUTHOR_FEED_RSS`` is used both for save location and URL.

.. data:: TAG_FEED_ATOM = None, i.e. no tag feed

   The location to save the tag Atom feed, including the ``{slug}``
   placeholder. [2]_

.. data:: TAG_FEED_ATOM_URL = None

   Relative URL of the tag Atom feed, including the ``{slug}`` placeholder.
   [2]_

.. data:: TAG_FEED_RSS = None, i.e. no RSS tag feed

   Relative URL to output the tag RSS feed, including the ``{slug}``
   placeholder. If not set, ``TAG_FEED_RSS`` is used both for save location and
   URL.

.. data:: FEED_MAX_ITEMS

   Maximum number of items allowed in a feed. Feed item quantity is
   unrestricted by default.

.. data:: RSS_FEED_SUMMARY_ONLY = True

   Only include item summaries in the ``description`` tag of RSS feeds. If set
   to ``False``, the full content will be included instead. This setting
   doesn't affect Atom feeds, only RSS ones.

If you don't want to generate some or any of these feeds, set the above
variables to ``None``.

.. [2] ``{slug}`` is replaced by name of the category / author / tag.


Pagination
==========

The default behaviour of Pelican is to list all the article titles along with a
short description on the index page. While this works well for small-to-medium
sites, sites with a large quantity of articles will probably benefit from
paginating this list.

You can use the following settings to configure the pagination.

.. data:: DEFAULT_ORPHANS = 0

   The minimum number of articles allowed on the last page. Use this when you
   don't want the last page to only contain a handful of articles.

.. data:: DEFAULT_PAGINATION = False

   The maximum number of articles to include on a page, not including orphans.
   False to disable pagination.

.. data:: PAGINATED_TEMPLATES = {'index': None, 'tag': None, 'category': None, 'author': None}

   The templates to use pagination with, and the number of articles to include
   on a page. If this value is ``None``, it defaults to ``DEFAULT_PAGINATION``.

.. data:: PAGINATION_PATTERNS = (
      (1, '{name}{extension}', '{name}{extension}'),
      (2, '{name}{number}{extension}', '{name}{number}{extension}'),
  )

   A set of patterns that are used to determine advanced pagination output.


Using Pagination Patterns
-------------------------

By default, pages subsequent to ``.../foo.html`` are created as
``.../foo2.html``, etc. The ``PAGINATION_PATTERNS`` setting can be used to
change this. It takes a sequence of triples, where each triple consists of::

  (minimum_page, page_url, page_save_as,)

For ``page_url`` and ``page_save_as``, you may use a number of variables.
``{url}`` and ``{save_as}`` correspond respectively to the ``*_URL`` and
``*_SAVE_AS`` values of the corresponding page type (e.g. ``ARTICLE_SAVE_AS``).
If ``{save_as} == foo/bar.html``, then ``{name} == foo/bar`` and ``{extension}
== .html``. ``{base_name}`` equals ``{name}`` except that it strips trailing
``/index`` if present. ``{number}`` equals the page number.

For example, if you want to leave the first page unchanged, but place
subsequent pages at ``.../page/2/`` etc, you could set ``PAGINATION_PATTERNS``
as follows::

  PAGINATION_PATTERNS = (
      (1, '{url}', '{save_as}`,
      (2, '{base_name}/page/{number}/', '{base_name}/page/{number}/index.html'),
  )


Translations
============

Pelican offers a way to translate articles. See the :doc:`Content <content>`
section for more information.

.. data:: DEFAULT_LANG = 'en'

   The default language to use.

.. data:: ARTICLE_TRANSLATION_ID = 'slug'

   The metadata attribute(s) used to identify which articles are translations
   of one another. May be a string or a collection of strings. Set to ``None``
   or ``False`` to disable the identification of translations.

.. data:: PAGE_TRANSLATION_ID = 'slug'

   The metadata attribute(s) used to identify which pages are translations of
   one another. May be a string or a collection of strings. Set to ``None`` or
   ``False`` to disable the identification of translations.

.. data:: TRANSLATION_FEED_ATOM = 'feeds/all-{lang}.atom.xml'

   The location to save the Atom feed for translations. [3]_

.. data:: TRANSLATION_FEED_ATOM_URL = None

   Relative URL of the Atom feed for translations, including the ``{lang}``
   placeholder. [3]_ If not set, ``TRANSLATION_FEED_ATOM`` is used both for
   save location and URL.

.. data:: TRANSLATION_FEED_RSS = None, i.e. no RSS

   Where to put the RSS feed for translations.

.. data:: TRANSLATION_FEED_RSS_URL = None

   Relative URL of the RSS feed for translations, including the ``{lang}``
   placeholder. [3]_ If not set, ``TRANSLATION_FEED_RSS`` is used both for save
   location and URL.

.. [3] {lang} is the language code


Ordering content
================

.. data:: NEWEST_FIRST_ARCHIVES = True

   Order archives by newest first by date. (False: orders by date with older
   articles first.)

.. data:: REVERSE_CATEGORY_ORDER = False

   Reverse the category order. (True: lists by reverse alphabetical order;
   default lists alphabetically.)

.. data:: ARTICLE_ORDER_BY = 'reversed-date'

   Defines how the articles (``articles_page.object_list`` in the template) are
   sorted. Valid options are: metadata as a string (use ``reversed-`` prefix
   the reverse the sort order), special option ``'basename'`` which will use
   the basename of the file (without path) or a custom function to extract the
   sorting key from articles. The default value, ``'reversed-date'``, will sort
   articles by date in reverse order (i.e. newest article comes first).

.. data:: PAGE_ORDER_BY = 'basename'

   Defines how the pages (``pages`` variable in the template) are sorted.
   Options are same as ``ARTICLE_ORDER_BY``.  The default value, ``'basename'``
   will sort pages by their basename.



Themes
======

Creating Pelican themes is addressed in a dedicated section (see
:ref:`theming-pelican`). However, here are the settings that are related to
themes.

.. data:: THEME

   Theme to use to produce the output. Can be a relative or absolute path to a
   theme folder, or the name of a default theme or a theme installed via
   ``pelican-themes`` (see below).

.. data:: THEME_STATIC_DIR = 'theme'

   Destination directory in the output path where Pelican will place the files
   collected from `THEME_STATIC_PATHS`. Default is `theme`.

.. data:: THEME_STATIC_PATHS = ['static']

   Static theme paths you want to copy. Default value is `static`, but if your
   theme has other static paths, you can put them here. If files or directories
   with the same names are included in the paths defined in this settings, they
   will be progressively overwritten.

.. data:: THEME_TEMPLATES_OVERRIDES = []

   A list of paths you want Jinja2 to search for templates before searching the
   theme's ``templates/`` directory.  Allows for overriding individual theme
   template files without having to fork an existing theme.  Jinja2 searches in
   the following order: files in ``THEME_TEMPLATES_OVERRIDES`` first, then the
   theme's ``templates/``.

   You can also extend templates from the theme using the ``{% extends %}``
   directive utilizing the ``!theme`` prefix as shown in the following example:

   .. parsed-literal::

      {% extends '!theme/article.html' %}

.. data:: CSS_FILE = 'main.css'

   Specify the CSS file you want to load.

By default, two themes are available. You can specify them using the ``THEME``
setting or by passing the ``-t`` option to the ``pelican`` command:

* notmyidea
* simple (a synonym for "plain text" :)

There are a number of other themes available at
https://github.com/getpelican/pelican-themes. Pelican comes with
:doc:`pelican-themes`, a small script for managing themes.

You can define your own theme, either by starting from scratch or by
duplicating and modifying a pre-existing theme. Here is :doc:`a guide on how to
create your theme <themes>`.

Following are example ways to specify your preferred theme::

    # Specify name of a built-in theme
    THEME = "notmyidea"
    # Specify name of a theme installed via the pelican-themes tool
    THEME = "chunk"
    # Specify a customized theme, via path relative to the settings file
    THEME = "themes/mycustomtheme"
    # Specify a customized theme, via absolute path
    THEME = "/home/myuser/projects/mysite/themes/mycustomtheme"

The built-in ``notmyidea`` theme can make good use of the following settings.
Feel free to use them in your themes as well.

.. data:: SITESUBTITLE

   A subtitle to appear in the header.

.. data:: DISQUS_SITENAME

   Pelican can handle Disqus comments. Specify the Disqus sitename identifier
   here.

.. data:: GITHUB_URL

   Your GitHub URL (if you have one). It will then use this information to
   create a GitHub ribbon.

.. data:: GOOGLE_ANALYTICS

   Set to ``UA-XXXXX-Y`` Property's tracking ID to activate Google Analytics.

.. data:: GA_COOKIE_DOMAIN

   Set cookie domain field of Google Analytics tracking code. Defaults to
   ``auto``.

.. data:: GOSQUARED_SITENAME

   Set to 'XXX-YYYYYY-X' to activate GoSquared.

.. data:: MENUITEMS

   A list of tuples (Title, URL) for additional menu items to appear at the
   beginning of the main menu.

.. data:: PIWIK_URL

   URL to your Piwik server - without 'http://' at the beginning.

.. data:: PIWIK_SSL_URL

   If the SSL-URL differs from the normal Piwik-URL you have to include this
   setting too. (optional)

.. data:: PIWIK_SITE_ID

   ID for the monitored website. You can find the ID in the Piwik admin
   interface > Settings > Websites.

.. data:: LINKS

   A list of tuples (Title, URL) for links to appear on the header.

.. data:: SOCIAL

   A list of tuples (Title, URL) to appear in the "social" section.

.. data:: TWITTER_USERNAME

   Allows for adding a button to articles to encourage others to tweet about
   them. Add your Twitter username if you want this button to appear.

.. data:: LINKS_WIDGET_NAME

   Allows override of the name of the links widget.  If not specified, defaults
   to "links".

.. data:: SOCIAL_WIDGET_NAME

   Allows override of the name of the "social" widget.  If not specified,
   defaults to "social".

In addition, you can use the "wide" version of the ``notmyidea`` theme by
adding the following to your configuration::

    CSS_FILE = "wide.css"


Logging
=======

Sometimes, a long list of warnings may appear during site generation. Finding
the **meaningful** error message in the middle of tons of annoying log output
can be quite tricky. In order to filter out redundant log messages, Pelican
comes with the ``LOG_FILTER`` setting.

``LOG_FILTER`` should be a list of tuples ``(level, msg)``, each of them being
composed of the logging level (up to ``warning``) and the message to be
ignored. Simply populate the list with the log messages you want to hide, and
they will be filtered out.

For example::
    
   import logging
   LOG_FILTER = [(logging.WARN, 'TAG_SAVE_AS is set to False')]

It is possible to filter out messages by a template. Check out source code to
obtain a template.

For example::

   import logging
   LOG_FILTER = [(logging.WARN, 'Empty alt attribute for image %s in %s')]

.. Warning::

   Silencing messages by templates is a dangerous feature. It is possible to
   unintentionally filter out multiple message types with the same template
   (including messages from future Pelican versions). Proceed with caution.

.. note::

    This option does nothing if ``--debug`` is passed.

.. _reading_only_modified_content:


Reading only modified content
=============================

To speed up the build process, Pelican can optionally read only articles and
pages with modified content.

When Pelican is about to read some content source file:

1. The hash or modification time information for the file from a
   previous build are loaded from a cache file if ``LOAD_CONTENT_CACHE`` is
   ``True``. These files are stored in the ``CACHE_PATH`` directory.  If the
   file has no record in the cache file, it is read as usual.
2. The file is checked according to ``CHECK_MODIFIED_METHOD``:

    - If set to ``'mtime'``, the modification time of the file is
      checked.
    - If set to a name of a function provided by the ``hashlib``
      module, e.g. ``'md5'``, the file hash is checked.
    - If set to anything else or the necessary information about the
      file cannot be found in the cache file, the content is read as usual.

3. If the file is considered unchanged, the content data saved in a
   previous build corresponding to the file is loaded from the cache, and the
   file is not read.
4. If the file is considered changed, the file is read and the new
   modification information and the content data are saved to the cache if
   ``CACHE_CONTENT`` is ``True``.

If ``CONTENT_CACHING_LAYER`` is set to ``'reader'`` (the default), the raw
content and metadata returned by a reader are cached. If this setting is
instead set to ``'generator'``, the processed content object is cached. Caching
the processed content object may conflict with plugins (as some reading related
signals may be skipped) and the ``WITH_FUTURE_DATES`` functionality (as the
``draft`` status of the cached content objects would not change automatically
over time).

Checking modification times is faster than comparing file hashes, but it is not
as reliable because ``mtime`` information can be lost, e.g., when copying
content source files using the ``cp`` or ``rsync`` commands without the
``mtime`` preservation mode (which for ``rsync`` can be invoked by passing the
``--archive`` flag).

The cache files are Python pickles, so they may not be readable by different
versions of Python as the pickle format often changes. If such an error is
encountered, it is caught and the cache file is rebuilt automatically in the
new format. The cache files will also be rebuilt after the ``GZIP_CACHE``
setting has been changed.

The ``--ignore-cache`` command-line option is useful when the whole cache needs
to be regenerated, such as when making modifications to the settings file that
will affect the cached content, or just for debugging purposes. When Pelican
runs in autoreload mode, modification of the settings file will make it ignore
the cache automatically if ``AUTORELOAD_IGNORE_CACHE`` is ``True``.

Note that even when using cached content, all output is always written, so the
modification times of the generated ``*.html`` files will always change.
Therefore, ``rsync``-based uploading may benefit from the ``--checksum``
option.

.. _writing_only_selected_content:


Writing only selected content
=============================

When only working on a single article or page, or making tweaks to your theme,
it is often desirable to generate and review your work as quickly as possible.
In such cases, generating and writing the entire site output is often
unnecessary. By specifying only the desired files as output paths in the
``WRITE_SELECTED`` list, **only** those files will be written. This list can be
also specified on the command line using the ``--write-selected`` option, which
accepts a comma-separated list of output file paths. By default this list is
empty, so all output is written. See :ref:`site_generation` for more details.


Example settings
================

.. literalinclude:: ../samples/pelican.conf.py
    :language: python


.. _Jinja custom filters documentation: http://jinja.pocoo.org/docs/api/#custom-filters
.. _Jinja Environment documentation: http://jinja.pocoo.org/docs/dev/api/#jinja2.Environment
.. _Docutils Configuration: http://docutils.sourceforge.net/docs/user/config.html
