FROM nunenuh/pml:cpu-base-ubuntu18.04

LABEL maintainer="Lalu Erfandi Maula Yusnu <nunenuh@gmail.com>"

ENV PYTHON_VERSION=3.6
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/anaconda/bin:$PATH

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/anaconda && rm ~/anaconda.sh

RUN /opt/anaconda/bin/conda install -y python=$PYTHON_VERSION && \
    /opt/anaconda/bin/conda clean -tipsy && \
    ln -s /opt/anaconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh 

# set bashrc and activate conda
RUN echo ". /opt/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc
	# /opt/anaconda/bin/conda init bash && \

# End Conda

# Tini: https://github.com/krallin/tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]
# End Tini

RUN apt-get update && apt-get install -y build-essential libssl-dev
RUN pip install cmake

COPY ./start.sh /start.sh
RUN chmod +x /start.sh

CMD [ "/start.sh" ]
