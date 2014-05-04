# MIME Types

A __MIME Type__ defines an association between a file content and a name. In Web Server context this is useful when sending some content to the client and this last one needs to know before hand what kind of data is being send.

Everybody is aware about files extensions, for example __JPG__ or __JPEG__ represents a file that its content is a compressed digital image and the MIME Type associated is __image/jpeg__. Monkey supports to register all __MIME Types__ required and that lists exists on the configuration file __conf/monkey.mime__.

## monkey.mime

This configuration file store all known __MIME Types__, it's schema contains just one section named __[MIMETYPES]__ and each entry represents a mime type where the key is the file extension and the value the __MIME Type__, e.g:

```Python
[MIMETYPES]
    html text/html
    jpg image/jpeg
    png image/png
    js application/x-javascript
    css text/css
    xml text/xml
    gif image/gif
    flv video/x-flv
```

So every time Monkey receives a request for a file named __logo.jpg__, it will lookup into the __MIME Types__ list and return back the value __image/jpeg__ on it's HTTP response header __Content-Type__. When a resource cannot match a __MIME Type__ the server will send the mime type __text/plain__.

There is no limits for the number of __MIME Types__ registered, they just need to be there before the server starts.
