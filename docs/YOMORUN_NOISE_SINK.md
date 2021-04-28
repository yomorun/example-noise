# Quick reference (cont.)

- **Use it for**: https://github.com/yomorun/yomo
- **Where to file issues**: https://github.com/yomorun/yomo-sink-socketio-server-example/issues
- **Source of this description**: https://github.com/yomorun/yomo-sink-socketio-server-example
- **Related Projects**: https://github.com/yomorun/example-noise

# What is noise-sink?

This is part of the [example-noise](https://github.com/yomorun/example-noise), which describes how to write a [**noise-sink**](https://github.com/yomorun/yomo-sink-socketio-server-example) to provide query capability via websocker server.

![arch1.png](https://github.com/yomorun/example-noise/raw/main/docs/arch1.png?raw=true)

# How to use this image

## run the container

```bash
docker run --rm --name noise-sink -p 4141:4141 -p 8000:8000 yomorun/noise-sink:latest
```

# License

View [license information](https://github.com/yomorun/yomo/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [`noise-sink`](https://github.com/yomorun/yomo-sink-socketio-server-example)

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

