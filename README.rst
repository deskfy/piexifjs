Piexifjs
========

This is exactly the same as version 1.0.4 of hMatoba's piexifjs https://github.com/hMatoba/piexifjs, 
but returning the input image itself when it is an unsupported type, instead of throwing an exception

Thanks to hMotoba for the hard work! 

How to Use
----------

- :code:`var exifObj = piexif.load(jpegData)` - Get exif data as *object*. *jpegData* must be a *string* that starts with "\data:image/jpeg;base64,"(DataURL), "\\xff\\xd8", or "Exif".
- :code:`var exifStr = piexif.dump(exifObj)` - Get exif as *string* to insert into JPEG.
- :code:`piexif.insert(exifStr, jpegData)` - Insert exif into JPEG. If *jpegData* is DataURL, returns JPEG as DataURL. Else if *jpegData* is binary as *string*, returns JPEG as binary as *string*.
- :code:`piexif.remove(jpegData)` - Remove exif from JPEG. If *jpegData* is DataURL, returns JPEG as DataURL. Else if *jpegData* is binary as *string*, returns JPEG as binary as *string*.

Use with File API or Canvas API.

Example
-------

.. code:: html

    <input type="file" id="files" />
    <script src="/js/piexif.js" />
    <script>
    function handleFileSelect(evt) {
        var file = evt.target.files[0];
        
        var zeroth = {};
        var exif = {};
        var gps = {};
        zeroth[piexif.ImageIFD.Make] = "Make";
        zeroth[piexif.ImageIFD.XResolution] = [777, 1];
        zeroth[piexif.ImageIFD.YResolution] = [777, 1];
        zeroth[piexif.ImageIFD.Software] = "Piexifjs";
        exif[piexif.ExifIFD.DateTimeOriginal] = "2010:10:10 10:10:10";
        exif[piexif.ExifIFD.LensMake] = "LensMake";
        exif[piexif.ExifIFD.Sharpness] = 777;
        exif[piexif.ExifIFD.LensSpecification] = [[1, 1], [1, 1], [1, 1], [1, 1]];
        gps[piexif.GPSIFD.GPSVersionID] = [7, 7, 7, 7];
        gps[piexif.GPSIFD.GPSDateStamp] = "1999:99:99 99:99:99";
        var exifObj = {"0th":zeroth, "Exif":exif, "GPS":gps};
        var exifStr = piexif.dump(exifObj);

        var reader = new FileReader();
        reader.onload = function(e) {
            var inserted = piexif.insert(exifStr, e.target.result);

            var image = new Image();
            image.src = inserted;
            image.width = 200;
            var el = $("<div></div>").append(image);
            $("#resized").prepend(el);

        };
        reader.readAsDataURL(file);
    }
    
    document.getElementById('files').addEventListener('change', handleFileSelect, false);
    </script>

Dependency
----------

No dependency. Piexifjs just needs standard JavaScript environment.

Environment
-----------

Both client-side and server-side. Standard browsers(Tested on IE11, Opera28, and PhantomJS) and Node.js.

Issues
------

Give me details. Environment, code, input, output. I can do nothing with abstract.

License
-------

This software is released under the MIT License, see LICENSE.txt.
