---
layout: project
library: http
version: 3.x
title: File uploads
---

Uploaded files are represented with the help of the `Opis\Http\IUploadedFile` interface.
A default implementation of this interface is provided by the library's own `Opis\Http\UploadedFile` class.

The first thing you will probably want to do with an uploaded file, is to move it to a custom location. 
This can be easily accomplished with the help of the `moveToFile` method. 

```php
// Get the uploaded file
$picture = $request->file('profile_picture');

// Move it
$picture->moveToFile('/path/to/file.jpg');
```

You can also move a picture to a given stream, by using the `moveToStream` method. 
One of the use-cases for this method, is when you want to move the uploaded file to another server, for example.

```php
// Get the uploaded file
$picture = $request->file('profile_picture');

// To stream
$picture->moveToStream(new \Opis\Stream\Stream('ftp:///path/to/file.jpg', 'w'));
```

If the file was already moved and you call the `moveToStream` or `moveToFile` method, an exception
will pe thrown. Use the `wasMoved` method to check if a file was moved or not.

```php
if (!$picture->wasMoved()){
    // do something
}
```

You can also obtain a read-only stream to the uploaded file, by calling the `getStream` method.

```php
$stream = $picture->getStream();
```

## Meta-information

The interface defines several method that can be used to obtain meta information about an uploaded file.
To check if the file was uploaded without error, just use the `getError` method. 
More info on the meaning of the integer value returned by this method can be found [here][file_upload].

```php
if ($file->getError() !== UPLOAD_ERR_OK) {
    // something went wrong
}
```

Checking the file-size of an uploaded file is done by using the `getSize` method. The method returns a value
that express the file's number of bytes.

```php
if ($file->getSize() > 1024 * 1024) {
    // do something
}
```

Obtaining information about the file-name and the file's media type, is done by using the
`getClientFilename` and `getClientMediaType` methods.

```php
echo $file->getClientFilename();
echo $file->getClientMediaType();
```

[file_upload]: http://php.net/manual/en/features.file-upload.errors.php "Error Messages Explained" 
{:rel="nofollow" target="_blank"}
