Hey there analyst.

Tired of always running into errors when setting up volatility? Well i got you sorted.

I built this docker image for volatility 2.6.1 out of desparation, which addresses all the issues such as:

- `ImportError: No module named Crypto.Hash`
- `ImportError: No module named distorm3`

Most of the help i got was from this github issue [#771](https://github.com/volatilityfoundation/volatility/issues/771)

I've uploaded the image to my Docker hub. If you intend to run it on an arm device, get the `oste/volatility2:latest` image. You can also get an image that would run on your amd64 architecture, use the `oste/volatility2:amd64` image.

Simply run:

```bash
docker run -v /home/kali/dump.vmem:/opt/volatility/dump.vmem --rm oste/volatility2:latest -f /opt/volatility/dump.vmem imageinfo
```

where:

- `docker run`: _This command is used to start a new Docker container from a Docker image._

- `-v /home/kali/cases/dump.vmem:/opt/volatility` - The `-v` option is used to mount a volume. This part of the command is saying "Take the file located at `/home/kali/cases/dump.vmem` on the host system and make it accessible within the Docker container at `/opt/volatility/`". This is useful for when you want to work with files on your host system from within a Docker container.

- `--rm` - This option tells Docker to automatically remove the container when it exits. This is useful for keeping your system clean of stopped containers that you don't intend to use again.

- `oste/volatility2:latest` - This is the name of the Docker image to start the container from. 
  
- `-f /opt/volatility/dump.vmem imageinfo` - This is the command that will be run within the Docker container once it starts. In this case, it's running the Volatility tool with the -f option, which tells it to perform a forensic analysis on a specific file, and imageinfo, which is a Volatility command that provides information about the memory image.

Enjoy. 😁