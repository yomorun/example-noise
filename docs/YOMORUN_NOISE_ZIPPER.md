# Quick reference (cont.)

- **Use it for**: https://github.com/yomorun/yomo
- **Where to file issues**: https://github.com/yomorun/yomo-zipper-noise-example/issues
- **Source of this description**: https://github.com/yomorun/yomo-zipper-noise-example
- **Related Projects**: https://github.com/yomorun/example-noise

# What is noise-zipper?

This is part of the [example-noise](https://github.com/yomorun/example-noise), which describes how to write a [**noise-zipper**](https://github.com/yomorun/yomo-zipper-noise-example) to receive data from the source, and send it to the configured workflow unit, like [noise-flow](https://github.com/yomorun/yomo-flow-noise-example) and [noise-sink](https://github.com/yomorun/yomo-sink-socketio-server-example).

![arch1.png](https://github.com/yomorun/example-noise/raw/main/docs/arch1.png?raw=true)

# How to use this image

## run the container

```bash
docker run --rm --name noise-zipper -p 9999:9999 yomorun/noise-zipper:latest
```

Output: 

```text
Attaching to noise-zipper
noise-zipper     | 2021/04/26 14:39:28 Found 1 flows in zipper config
noise-zipper     | 2021/04/26 14:39:28 Flow 1: Noise Serverless on noise-flow:4242
noise-zipper     | 2021/04/26 14:39:28 Found 1 sinks in zipper config
noise-zipper     | 2021/04/26 14:39:28 Sink 1: Socket.io Server on noise-sink:4141
noise-zipper     | 2021/04/26 14:39:28 Running YoMo workflow...
noise-zipper     | 2021/04/26 14:39:28 ✅ Listening on 0.0.0.0:9999
noise-zipper     | 2021/04/26 14:43:32 ✅ Connect to Noise Serverless (noise-flow:4242) successfully.
noise-zipper     | 2021/04/26 14:44:33 ✅ Connect to Socket.io Server (noise-sink:4141) successfully.
```

## Customized workflows

If the embedded workflow does not meet the requirements, you can specify one：

```ba
docker run --rm --name noise-zipper -p 9999:9999 \
	-v $PWD/workflow.yaml:/go/src/app/workflow.yaml \
	yomorun/noise-zipper:latest 
```

# License

View [license information](https://github.com/yomorun/yomo/blob/master/LICENSE) for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in [`noise-source`](https://github.com/yomorun/yomo-source-noise-example)

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.

