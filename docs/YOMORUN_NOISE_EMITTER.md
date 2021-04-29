# Quick reference (cont.)

- **Use it for**: https://github.com/yomorun/yomo
- **Where to file issues**: https://github.com/yomorun/yomo-source-noise-emitter-example/issues
- **Source of this description**: https://github.com/yomorun/yomo-source-noise-emitter-example
- **Related Projects**: https://github.com/yomorun/example-noise

# What is noise-emitter?

This is part of the [example-noise](https://github.com/yomorun/example-noise), which describes how to write a [**noise-emitter**](https://github.com/yomorun/yomo-source-noise-emitter-example) to produce data for [noise-source](https://github.com/yomorun/yomo-source-noise-example).

![arch1.png](https://github.com/yomorun/example-noise/raw/main/docs/arch1.png?raw=true)

# How to use this image

## run the container

```bash
docker run --rm --name noise-emitter \
  -e YOMO_SOURCE_MQTT_BROKER_ADDR=tcp://{YOUR-NOISE-SOURCE-ADDR}:1883 \
  -e YOMO_SOURCE_MQTT_PUB_INTERVAL=500 \
  yomorun/noise-emitter:latest
```

- YOMO_SOURCE_MQTT_BROKER_ADDR: service-address of [noise-source](https://github.com/yomorun/yomo-source-noise-example).
- YOMO_SOURCE_MQTT_PUB_TOPIC: simulate the TOPIC used by MQTT.
- YOMO_SOURCE_MQTT_PUB_INTERVAL: frequency of production data. Unit is milliseconds.

# License

View [license information](https://github.com/yomorun/yomo/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [`noise-emitter`](https://github.com/yomorun/yomo-source-noise-emitter-example)

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

