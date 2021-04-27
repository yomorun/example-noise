# Quick reference (cont.)

- **Use it for**: https://github.com/yomorun/yomo
- **Where to file issues**: https://github.com/yomorun/yomo-source-noise-example/issues
- **Source of this description**: https://github.com/yomorun/yomo-source-noise-example
- **Related Projects**: https://github.com/yomorun/example-noise

# What is noise-source?

This is part of the [example-noise](https://github.com/yomorun/example-noise), which describes how to write a **[noise-source](https://github.com/yomorun/yomo-source-noise-example)** to receive data from the device, and send it to the back-end workflow engine(**[noise-zipper](https://github.com/yomorun/yomo-zipper-noise-example)**) after it has been transformed and encoded.

![arch1.png](https://github.com/yomorun/example-noise/raw/main/docs/arch1.png?raw=true)

# How to use this image

## run the container

```bash
docker run --rm --name noise-source -p 1883:1883 \
  -e YOMO_SOURCE_MQTT_ZIPPER_ADDR=192.168.108.100:9999 \
  -e YOMO_SOURCE_MQTT_SERVER_ADDR=0.0.0.0:1883 \
  yomorun/noise-source:latest
```

- YOMO_SOURCE_MQTT_ZIPPER_ADDR: Set the service address of the remote noise-zipper.
- YOMO_SOURCE_MQTT_SERVER_ADDR: Set the external service address of this noise-source.

# License

View [license information](https://github.com/yomorun/yomo/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [`noise-source`](https://github.com/yomorun/yomo-source-noise-example).

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

