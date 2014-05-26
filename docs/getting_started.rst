
Getting started
===============

A Stupeflix Factory demo is available at http://studio.stupeflix.com/factory/demo/.


Editing sessions
----------------

Stupeflix Factory works with editing sessions. One editing session ends with one produced video.

Every editing session goes like this:

1. Configure/Create a session
2. Display the editing interface
3. Get the produced video info


Configure a session
-------------------

POST your configuration to the following endpoint to create a session::

    https://studio.stupeflix.com/factory/v3/


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

``video_name``
    The name you want to give to your user's video (your user will be able to change it).

``export_profile``
    The video profile to use to export the video. It defines width, height, bitrate and codecs of the video to be produced. See `Stupeflix API supported formats <http://stupeflix-api.readthedocs.org/en/latest/resources/05_supported_coders_formats.html>`_ for a list of the available profiles. Default is ``youtube``.

``allow_preview``
    Defines if the factory allows the user to preview her video.
    Possible values are ``true`` (default) or ``false``.

``custom_css_url``
    The URL of an extra css file to include into the factory html pages. This url must be served over https if you choose to display your factory using https.
    
``export_label``
    The label to display for the export button, default is ``Export``.
            
``rendering_label``
    The label to display on the video rendering screen (progress bar), default is ``Please wait while Stupeflix.com creates your video...``.

``secret``
    Your Stupeflix API key secret is needed to authenticate your configuration call.
    Manage your Stupeflix API keys on the `Stupeflix Developers website <https://developer.stupeflix.com/>`_.

Advanced configuration parameters
`````````````````````````````````

``video_id``
    Re-configure an existing non-complete editing session. Supports all standard configuration parameters except for ``export_profile``.
    ``video_id`` must be a valid non-complete editing session identifier.

``remix``
    Clone an existing editing session.
    ``remix`` must be a valid editing session identifier.

``definition``
    Use the ``definition`` parameter to prepopulate the editor.
    The ``definition`` value must be a valid json project definition.

``custom_media_library_url``
    | The URL of a custom media library server. See :doc:`custom_media_library`.
    | If configured Stupeflix Factory will display the custom media library option alongside the others media importers.
    
``custom_media_library_name``
    The name to display for the custom media library option.

Response
````````
A successfull configuration call returns a JSON response like this::

    {
        "video_id": "12345"
        "video_name": "My Stupeflix Video",
        "status": "editing",
        "edit_url": "https://studio.stupeflix.com/factory/v3/edit/?video_id=12345"
        "created_at": "2013-12-17"
    }

``video_id``
    This editing session unique identifier.
    As a session ends being a video, the session parameter is called ``video_id``.
    That's the identifier your have to associate with your users if you want to keep track of which videos were created by which users.

``video_name``
    The name of the created video.

``status``
    The status of the editing session, possible values are ``editing``, ``rendering`` or ``complete``.

``edit_url``
    This editing session url, to display in an iframe.

``created_at``
    The iso format utc datetime when the editing session was created.


Display the editing interface
-----------------------------

Once you get an ``edit_url``, you have to display its content to your user.
You can either do it by redirecting your user's browser to this url or by setting this url as an iframe src::

    <iframe src="https://studio.stupeflix.com/factory/v3/edit/?video_id=12345"
        width="960" height="600" scrolling="no" frameborder="no"></iframe>


Get the produced video info
---------------------------

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


Get a session' status
---------------------

Session' statuses are available anytime at the following endpoint, passing in ``video_id`` as your session identifier::

    https://studio.stupeflix.com/factory/v3/status/?video_id=12345
