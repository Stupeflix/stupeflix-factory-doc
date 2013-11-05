
Getting started
===============

A Stupeflix Factory demo is available at http://studio.stupeflix.com/factory/demo/.


Editing sessions
----------------

Stupeflix Factory works with editing sessions. One editing session ends with one produced video.

Every editing session goes like this:
1. Configure a session
2. Display the session's editing interface
3. Get the session's output


Configure a session
-------------------

Each factory has its own https endpoint, in the form of ``https://studio.stupeflix.com/factory/FACTORY_ID/``.
To configure an editing session you need to http POST your configuration to this endpoint.


Configuration parameters
````````````````````````

Stupeflix Factory works like basic html forms with ``action``, ``method`` and ``target`` parameters defining how the factory will call you server back.

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

``allow_preview``
    Defines if the factory allows the user to preview her video.
    Possible values are ``true`` (default) or ``false``.

``custom_css_url``
    The URL of an extra css file to include into the factory html pages.
    
``export_label``
    The label to display for the export button, default is ``Export``.
            
``rendering_label``
    The label to display on the video rendering screen (progress bar), default is ``Please wait while Stupeflix.com creates your video...``.

``custom_media_library_url``
    | The URL of a custom media library server. See :doc:`custom_media_library`.
    | If configured Stupeflix Factory will display the custom media library option alongside the others media importers.
    
``custom_media_library_name``
    The name to display for the custom media library option.

``definition``
    Use the ``definition`` parameter to prepopulate the editor.
    The ``definition`` value must be a valid json project definition.

``secret``
    Your factory has been associated with a Stupeflix API key.
    Use your API key secret to authenticate your configuration call.


Response
````````
A successfull configuration call returns a response like this::

    {
        "factory_id": "ABCDEF",
        "video_id": "ABCDEF/12345"
        "edit_video_url": "https://studio.stupeflix.com/factory/ABCDEF/edit/?video_id=ABCDEF%2F12345"
    }


``factory_id``
    Your Stupeflix Factory unique identifier.

``video_id``
    This editing session unique identifier.
    That's the identifier your have to associate with your users if you want to keep track of which videos were created by which users.

``edit_video_url``
    This editing session url, to display in an iframe.


Additionnal configuration parameters
````````````````````````````````````

``video_id``
    Resume an existing editing session.
    ``video_id`` must be a valid editing session identifier.

``remix``
    Clone an existing editing session (the video in it may be exported or not).
    ``remix`` must be a valid editing session identifier.

    
Display the session's editing interface
----------------------------------------

Once you get an ``edit_video_url``, you have to display its content to your user.
You can either do it by redirecting your user's browser to this url or by setting this url as an iframe src::

    <iframe src="https://studio.stupeflix.com/factory/ABCDEF/edit/?video_id=ABCDEF%2F12345"
        width="960" height="600" scrolling="no" frameborder="no"></iframe>


Get the session's output
------------------------

When your user's video is ready, Stupeflix Factory will call your server back, respecting your 
``action``, ``method`` and ``target`` configuration with the following data:

``video_id``
    This editing session unique identifier.
    
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


Static configuration
--------------------

Stupeflix Factory supports a number of server side configuration parameters.
For now these parameters can only be set by Stupeflix staff.

``video_name``
    The default name for new videos, default is ``My Stupeflix Video``.

``export_profile``
    The video format to use to export the user's video. See `Stupeflix API supported formats <http://stupeflix-api.readthedocs.org/en/latest/resources/05_supported_coders_formats.html>`_.
            
``upload_target``
    | Where to upload the exported video. 
    | Stupeflix Factory supports Youtube, Facebook, Dailymotion, S3, FTP, HTTP POST and HTTP PUT uploads.
    
