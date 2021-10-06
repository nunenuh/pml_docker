FROM nunenuh/pml:cpu-base-ubuntu20.04

LABEL maintainer="Lalu Erfandi Maula Yusnu <nunenuh@gmail.com>"

ENV PYTHON_VERSION=3.8
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/anaconda/bin:$PATH

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/anaconda && rm ~/anaconda.sh

RUN /opt/anaconda/bin/conda install -y python=$PYTHON_VERSION pytorch torchvision torchaudio cpuonly -c pytorch-lts && \
    /opt/anaconda/bin/conda clean -tipsy && \
    ln -s /opt/anaconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh 

# set bashrc and activate conda
RUN echo ". /opt/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && \
	/opt/anaconda/bin/conda init bash && \
    echo "conda activate base" >> ~/.bashrc

# End Conda

RUN apt-get update && apt-get install -y build-essential libssl-dev
RUN pip install cmake
# RUN systemctl start snapd
# RUN /usr/bin/snap install cmake --classic

#COPY ./conda.yml /conda.yml
#RUN chmod +x /conda.yml
#RUN /opt/anaconda/bin/conda env update -f conda.yml --prune 

COPY ./req.txt /req.txt
RUN chmod +x /req.txt
RUN pip install -r req.txt --no-cache-dir

RUN apt update && apt install software-properties-common -y
RUN add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main" && apt update \
    && apt install libjasper1 libjasper-dev

RUN apt update && apt install -y cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev \
    python-dev python-numpy libtbb2 \
    libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev \
    libatlas-base-dev libjasper-dev python3-pyqt5

# Tini: https://github.com/krallin/tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]
# End Tini

COPY ./start.sh /start.sh
RUN chmod +x /start.sh

CMD [ "/start.sh" ]
