# Following

## Camera Streaming
Camera streams video via rtsp/udp

## Camera Definition
The camera must enable http server function to send .xml file to QGC (default port: 5000)

## Connection
The camera connects to QGC via udp connection standard and sends heartbeat message. (Ex: udp:0.0.0.0:14550)

### Camera Operations
- Get Camera Information using `CAMERA_INFORMATION (259)` message:
  - After QGC receives the heartbeat message, it will send `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `259` message. The camera should return a `MAV_RESULT` and then `259` message.
    
- Sync between camera and QGC using `CAMERA_SETTINGS (260)` message:
  - The QGC would send the `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `260` message. The camera should return a `MAV_RESULT` and then send `260` message.
    
- Storage Status using `STORAGE_INFORMATION (261)` message:
  - Before capturing images and/or videos, a QGC should query the storage status to determine if the camera has enough free space for these operations.
  - The QGC would send the `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `STORAGE_INFORMATION (261)` message. The camera should return a `MAV_RESULT` and then send `261` message.
  - For formatting (or erasing), the QGC will send a `MAV_CMD_STORAGE_FORMAT (526)` message.

- Set Mode (Image / Video / Survey) using `MAV_CMD_SET_CAMERA_MODE (530)` mesage:
  - The QGC would send the `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `530` message. The camera should return a `MAV_RESULT`.
    
- Set Zoom using `MAV_CMD_SET_CAMERA_ZOOM (531)` message:
  - The QGC would send the `531` to set zoom in camera. Once done, the camera must respond to message `CAMERA_SETTINGS (260)` to QGC.
    
- Photo using `MAV_CMD_IMAGE_START_CAPTURE (2000)` message:
  - Confirm that the camera is ready to take images before allowing the user to request image capture:
    - The QGC would send the `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `CAMERA_CAPTURE_STATUS (262)` message. The camera should return a `MAV_RESULT` and then `CAMERA_CAPTURE_STATUS`.
  - The QGC would send the `2000` message to take pictures. Every time a photo is taken, the camera sends the captured image information to QGC using the `CAMERA_IMAGE_CAPTURED (263)` message.
    
- Video using `MAV_CMD_VIDEO_START_CAPTURE (2500)` and `MAV_CMD_VIDEO_START_CAPTURE (2501)` message:
  - The QGC would send the `2500` message and `2501` message to start/stop recording process.

- Stream using `VIDEO_STREAM_INFORMATION (269)` message:
  - The QGC would send the `MAV_CMD_REQUEST_MESSAGE (512)` message asking for `269` message. The camera should return a `MAV_RESULT` and then send `269` message.
  
- Reset Setting using `MAV_CMD_RESET_CAMERA_SETTINGS (529)` message:
  - The QGC would send the `529` to reset setting of camera.

- Set parram in .xml file by QGC using `PARAM_EXT_SET (323)` message:
  -  the GCS will send a `323` message with the id, value in .xml file and response by `PARAM_EXT_ACK (324)` message.
  -  `PARAM_EXT_REQUEST_LIST (321)` message is used for request all parameters of this component. All parameters should be emitted in response as `PARAM_EXT_VALUE (322)`.


