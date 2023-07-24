FROM ubuntu:22.10

RUN apt-get update -y && \
    apt-get install -y git curl nano vim wget software-properties-common python3 python3-pip && \
    apt-get update -y 

WORKDIR /opt

RUN git clone https://github.com/volatilityfoundation/volatility3.git

WORKDIR /opt/volatility3

RUN pip3 install -r requirements.txt

ENTRYPOINT ["python3", "vol.py"]