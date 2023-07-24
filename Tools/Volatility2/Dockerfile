FROM ubuntu:22.10

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y git curl nano vim python2.7 wget pcregrep libpcre++-dev python2-dev build-essential libdistorm3-dev yara libraw1394-11 libcapstone-dev capstone-tool tzdata \
    && curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py \
    && python2 get-pip.py \
    && pip2 install pycrypto distorm3 \
    && echo 'alias volatility="python2 /opt/volatility/vol.py"' >> /root/.bashrc \
    && git clone https://github.com/volatilityfoundation/volatility.git /opt/volatility

WORKDIR /opt/volatility

ENTRYPOINT ["python2", "vol.py"]