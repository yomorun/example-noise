# Quick reference (cont.)

- **Use it for**: https://github.com/yomorun/yomo
- **Where to file issues**: https://github.com/yomorun/yomo-flow-noise-example/issues
- **Source of this description**: https://github.com/yomorun/yomo-flow-noise-example
- **Related Projects**: https://github.com/yomorun/example-noise

# What is noise-flow?

This is part of the [example-noise](https://github.com/yomorun/example-noise), which describes how to write a [**noise-flow**](https://github.com/yomorun/yomo-flow-noise-example) to receive data from the source, and send it to the configured workflow unit, like [noise-flow](https://github.com/yomorun/yomo-flow-noise-example) and [noise-sink](https://github.com/yomorun/yomo-sink-socketio-server-example).

![arch1.png](https://github.com/yomorun/example-noise/raw/main/docs/arch1.png?raw=true)

# How to use this image

## run the container

```bash
docker run --rm --name noise-flow -p 4242:4242 yomorun/noise-flow:latest
```

# License

View [license information](https://github.com/yomorun/yomo/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [`noise-flow`](https://github.com/yomorun/yomo-flow-noise-example)

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

