
Getting started
===============

| A Stupeflix Factory has an unique URL in the form of ``http://studio.stupeflix.com/factory/FACTORY_ID/``.
| A demo Stupeflix Factory is available to try at http://studio.stupeflix.com/factory/demo/.

Factory parameters
------------------

Stupeflix Factory works like a basic html form with ``action``, ``method`` and ``target`` parameters defining how the factory will call you server back.

``action``
    The URL of the program that will process the information submitted by the factory.
    If this value is not specified the factory will submit to a debug page.
    
``method``
    The HTTP method to use to submit the factory information to the ``action`` URL. Possible values are ``get`` (default) or ``post``.
      
``target``
    A name or keyword indicating where to display the response returned by the ``action`` URL.
        * ``_self``: Load the response into the same frame as the factory.
        * ``_blank``: Load the response into a new unnamed window.
        * ``_parent``: Load the response into the factory parent frame. If there is no parent, this option behaves the same way as _self.
        * ``_top`` (default): Load the response into the factory top-level parent frame. If there is no parent, this option behaves the same way as _self.

``video_id``
    You may specify a ``video_id`` parameter to create an identified project or to re-open a previously created project.
    The ``video_id`` value must be alphanumerical with a max length of 20.

``definition``
    Use the ``definition`` parameter to prepopulate the editor.
    The ``definition`` value must be a valid json project definition. If submitted with a ``video_id`` parameter the definition will erase the associated project definition.

| Add these parameters as GET parameters to your factory URL and open it into the window/iframe of your choice.
| Stupeflix Factory will allow your user to compose, preview and export a single video.

Factory response
----------------

Once your user's video is ready Stupeflix Factory will call your server back with the following data:

``video_id``
    Stupeflix internal unique video identifier.
    
``video_name``
    The name your user gave to her video.

``video_url``
    The URL of the exported video file.
        
``thumb_url``
    An URL pointing to a thumbnail of the exported video.

``hres``
    The horizontal (x) resolution of the exported video and thumbnail.
    
``vres``
    The vertical (y) resolution of the exported video and thumbnail.

``error``
    If the factory encountered an error it will show up here.

Static configuration
--------------------

| Stupeflix Factory supports a number of server side configuration parameters.
| For now these parameters can only be set by Stupeflix staff.

``allow_preview``
    Defines if the factory allows the user to preview her video.
    Possible values are ``true`` or ``false``.

``custom_css_url``
    The URL of an extra css file to include into the factory html pages.
    
``custom_media_library_url``
    | The URL of a custom media library server. See :doc:`custom_media_library`.
    | If configured Stupeflix Factory will display the custom media library option alongside the others media importers.
    
``custom_media_library_name``
    The name to display for the custom media library option.

``export_label``
    The label to display for the export button, default is ``Export``.
    
``export_profile``
    The video format to use to export the user's video. See `Stupeflix API supported formats <http://stupeflix-api.readthedocs.org/en/latest/resources/05_supported_coders_formats.html>`_.
        
``rendering_label``
    The label to display on the video rendering screen (progress bar), default is ``Please wait while Stupeflix.com creates your video...``.
    
``upload_target``
    | Where to upload the exported video. 
    | Stupeflix Factory supports Youtube, Facebook, Dailymotion, S3, FTP, HTTP POST and HTTP PUT uploads.
    
``video_name``
    The default name for new videos, default is ``My Stupeflix Video``.
