.. include:: headings.inc


.. _filesystem overview:

========================================
|phoenix_title|  **FileSystem Overview**
========================================

The wxHTML library uses a **virtual** file systems mechanism similar
to the one used in Midnight Commander, Dos Navigator, FAR or almost
any modern file manager.

It allows the user to access data stored in archives as if they were
ordinary files. On-the-fly generated files that exist only in memory
are also supported.



Classes
-------

Three classes are used in order to provide virtual file systems
mechanism:

* The :ref:`wx.FSFile` class provides information about opened file
  (name, input stream, mime type and anchor).

* The :ref:`wx.FileSystem` class is the interface. Its main methods are
  :meth:`wx.FileSystem.ChangePathTo` and
  :meth:`wx.FileSystem.OpenFile`. This class is most often used by the
  end user.

* The :ref:`wx.FileSystemHandler` is the core of virtual file systems
  mechanism. You can derive your own handler and pass it to the VFS
  mechanism. You can derive your own handler and pass it to the
  :meth:`wx.FileSystem.AddHandler` method. In the new handler you only
  need to override the :meth:`wx.FileSystemHandler.OpenFile` and
  :meth:`wx.FileSystemHandler.CanOpen` methods.



Locations
---------

Locations (aka filenames aka addresses) are constructed from four
parts:

* **protocol** - handler can recognize if it is able to open a file by
  checking its protocol. Examples are ``"http"``, ``"file"`` or
  ``"ftp"``.

* **right location** - is the name of file within the protocol. In
  ``"http://www.wxwidgets.org/index.html"`` the right location is
  ``"//www.wxwidgets.org/index.html"``.

* **anchor** - an anchor is optional and is usually not present. In
  ``"index.htm#chapter2"`` the anchor is ``"chapter2"``.

* **left location** - this is usually an empty string. It is used by
  'local' protocols such as ZIP. See the :ref:`Combined Protocols
  <combined protocols>` paragraph for details.


.. _combined protocols:

Combined Protocols
------------------

The left location precedes the protocol in the URL string.

It is not used by global protocols like HTTP but it becomes handy when
nesting protocols - for example you may want to access files in a ZIP
archive:

``file:archives/cpp_doc.zip#zip:reference/fopen.htm#syntax``

In this example, the protocol is ``"zip"``, right location is
``"reference/fopen.htm"``, anchor is ``"syntax"`` and left location is
``file:archives/cpp_doc.zip``.

There are two protocols used in this example: "zip" and "file".


.. _list of available handlers:

File Systems Included in wxHTML
-------------------------------

The following virtual file system handlers are part of wxPython so far:

* :ref:`wx.ArchiveFSHandler`: A handler for archives such as zip and
  tar. URLs examples: ``"archive.zip#zip:filename"``,
  ``"archive.tar.gz#gzip:#tar:filename"``.

* :ref:`wx.FilterFSHandler`: A handler for compression schemes such as
  gzip. URLs are in the form, e.g.: ``"document.ps.gz#gzip:"``.

* :ref:`wx.InternetFSHandler`: A handler for accessing documents via HTTP
  or FTP protocols.

* :ref:`wx.MemoryFSHandler`: This handler allows you to access data
  stored in memory (such as bitmaps) as if they were regular
  files. See :ref:`wx.MemoryFSHandler` for details.  URL is prefixed with
  memory:, e.g. ``"memory:myfile.htm"``.


In addition, :ref:`FileSystem` itself can access local files.



Initializing file system handlers
---------------------------------

Use :meth:`wx.FileSystem.AddHandler` to initialize a handler, for
example::

	def OnInit(self):
	    wx.FileSystem.AddHandler(wx.MemoryFSHandler())


