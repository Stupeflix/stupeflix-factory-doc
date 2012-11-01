How to implement a Custom Media Library Server
==============================================

Requests
--------

Stupeflix Factory will query the configured ``custom_media_library_url`` each time an user wants to access the custom library content.

Root level content
^^^^^^^^^^^^^^^^^^

To get a 50 items list starting at the item n°0, Stupeflix Factory will issue a GET request to:

    ``http://custom_media_library_url?method=getList&offset=0&limit=50``

    GET parameters:
        ======  =======
        name    value  
        ======  =======
        method  getList
        offset  0      
        limit   50     
        ======  =======

to get the next 50 items the request will be:

    ``http://custom_media_library_url?method=getList&offset=50&limit=50``

    GET parameters:
        ======  =======
        name    value  
        ======  =======
        method  getList
        offset  50      
        limit   50     
        ======  =======

Folder content
^^^^^^^^^^^^^^

To get the 50 first items of the folder ``562436708``, Stupeflix Factory will issue a GET request to:

    ``http://custom_media_library_url?method=getList&id=562436708&offset=50&limit=50``

    GET parameters:
        ======  =========
        name    value  
        ======  =========
        method  getList
        id      562436708
        offset  0      
        limit   50     
        ======  =========

Responses
---------

custom_media_library_url server responses are expected in JSON format (http://www.json.org/) with an ``application/javascript`` Content-Type.

.. code-block:: javascript

    { 
      /* offset & limit values used for this response */
      "offset": 0,
      "limit": 50, 
 
      /* Items to display (array)
         supported types are folder, image, video */
 
      "items": [
 
        { 
          "type": "folder", 
          "id": "562436708",    /* Unique folder identifier on your server.
                                   Stupeflix Factory will issue a GET request to:
                                   http://custom_media_library_url?method=getList&id=562436708&offset=0&limit=50
                                   to get this folder content */
 
          "title": "Folder A",  /* Name to display */
          
          "thumb_url": "http://whatever/thumbnail/url.jpg", /* Url of the thumbnail to display */
          
          /* Optional properties */
          "num_entries": 10 // Number of entries in this folder */
        }, 
 
        {
          "type": "image",
          "title": "Image 1",
          "file_url": "http://image-1/url.jpg",
          "thumb_url": "http://image-1/thumbnail/url.jpg",
 
          /* Optional properties */
          "height": 800,    // Height of the image in pixels
          "width": 600      // Width of the image in pixels
        },
 
        {
          "type": "video",
          "title": "Video 1",
          "file_url": "http://video-1/url.mp4",
          "thumb_url": "http://video-1/thumbnail/url.jpg",
 
          /* Optional properties */
          "height": 640,    // Height of the video in pixels
          "width": 480,     // Width of the video in pixels
          "duration": 120   // Duration of the video in seconds
        }
 
      ], 
 
      /* Does the server have more items in this list ?
         If has_more is true, Stupeflix Factory will issue a new GET request with an incremented offset parameter */
      "has_more": false
    }

