## Frigate Config

As a prerequisite of running this project, you must set up Frigate to detect the 'bird' object in a video stream, and to send out snapshots. This also assumes you have setup a MQTT broker, like Mosquitto MQTT

Example Frigate Config Needed (This is purely for reference. This config assumes you have a CORAL TPU USB and Intel IGPU using VAAPI and most likely will not work if you copy and paste. Please tune it to your Frigate & MQTT configuration. See the full Frigate configuration file documentation [here](https://docs.frigate.video/configuration/))

## Minimum Camera Example

```yaml
mqtt:
  enabled: true
  host: 192.168.1.10

cameras:
  bird_camera:
    ffmpeg:
      inputs:
        - path: rtsp://username:password@camera-address/stream
          roles:
            - detect

    detect:
      enabled: true

    objects:
      track:
        - bird
```

!!! note

    This is a minimal example only. Replace the MQTT address, camera name,
    credentials and RTSP path with values appropriate for your environment.

## Recommended WAMF Example

```yaml
mqtt:
  enabled: true
  host: 192.168.1.10
  port: 1883

detectors:
  coral:
    type: edgetpu
    device: USB # This will be configured to your specific detector

cameras:
  bird_camera:
    ffmpeg:
      inputs:
        - path: rtsp://username:password@camera-address/detect-stream
          roles:
            - detect

        - path: rtsp://username:password@camera-address/record-stream
          roles:
            - record

    detect:
      enabled: true
      width: 640
      height: 480
      fps: 7

    objects:
      track:
        - bird

    snapshots:
      enabled: true

    record:
      enabled: true
```

!!! Note

    If you don't have a Coral TPU, replace the detectors section with the appropriate configuration for your hardware. Refer to the Frigate documentation for supported detector types.


The Frigate camera name must match the camera name configured in WAMF.

For example, a Frigate camera named:

`bird_camera`

must use the same identifier in the WAMF configuration.