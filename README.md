# Dynamsoft Camera SDK

## Overview
[Dynamsoft Camera SDK](https://www.dynamsoft.com/Products/dynamsoft-webcam-sdk.aspx) provides JavaScript APIs that enable you to easily capture images and documents from USB Video Class (UVC) compatible webcams within a browser. It supports document edge detection from a video stream and processing features including perspective correction, noise removal, contrast, brightness, and color filter (convert to a colored/grey document).

## Features
* Programming languages: JavaScript, CSS, HTML.
* Camera Settings: Developers can have complete control over a camera, e.g., exposure, iris, auto focus, backlight compensation, brightness, saturation, sharpness, gamma, contrast, white balance temperature, gain
* Image Editing: Rotates, flips, mirrors, cuts, deletes or crops an image, etc.
* Upload Images: Uploads specified images to an HTTP server. Both synch and async modes are supported. Sets text fields in the web form which will be sent to the server along with the images.
* [More](https://www.dynamsoft.com/Products/webcam-sdk-features.aspx)

## Documentation

* [Developer's Guide](https://www.dynamsoft.com/Products/webcam-sdk-resource.aspx)
* [Sample Code](https://www.dynamsoft.com/Downloads/dynamsoft-webcam-sdk-sample-download.aspx)

## Quick Start

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello World</title>
    <script type="text/javascript" src="https://www.dynamsoft.com/library/dcs/dynamsoft.camera.min.js"> </script>
</head>

<body>
    <h1>Dynamsoft Webcam SDK: Hello World</h1>
    <select size="1" id="source" width="220px" onchange="onSourceChanged()"></select>
    <input type="button" value="Capture" onclick="onCapture();" />

    <div id="video-container"></div>
    <div id="image-container"></div>

    <script type="text/javascript">
        var dcsObject, imageViewer;var width = 480,height = 320;

        function onInitSuccess(videoViewerId, imageViewerId) {

            dcsObject = dynamsoft.dcsEnv.getObject(videoViewerId);
            dcsObject.videoviewer.setWidth(width);
            dcsObject.videoviewer.setHeight(height);
            imageViewer = dcsObject.getImageViewer(imageViewerId);
            imageViewer.ui.setWidth(width);
            imageViewer.ui.setHeight(height);
            var cameraList = dcsObject.camera.getCameraList();
            for (var i = 0; i < cameraList.length; i++) {
                document.getElementById("source").options.add(new Option(cameraList[i], i));
            }

            if (cameraList.length > 0) {
                dcsObject.camera.takeCameraOwnership(cameraList[0]);
                dcsObject.camera.playVideo();
            } else {
                alert('No camera is connected.');
            }
        }

        function onInitFailure(errorCode, errorString) {alert('Init failed: ' + errorString);}

        function onSourceChanged() {
            if (!dcsObject) return;
            var source = document.getElementById("source");
            var camera = source.options[source.selectedIndex].text;
            dcsObject.camera.takeCameraOwnership(camera);
            dcsObject.camera.playVideo();
        }

        function onCapture() {
            if (!dcsObject) return;
            dcsObject.camera.captureImage('image-container');
            if (dcsObject.getErrorCode() !== EnumDCS_ErrorCode.OK) {
                alert('Capture error: ' + dcsObject.getErrorString());
            }
        }

        dynamsoft.dcsEnv.init('video-container', 'image-container', onInitSuccess, onInitFailure);

        window.onbeforeunload = function() {if (dcsObject) dcsObject.destroy();};
    </script>
</body>
</html>
```
