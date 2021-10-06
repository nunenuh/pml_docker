FROM nunenuh/pml:cpu-base-ubuntu20.04

LABEL maintainer="Lalu Erfandi Maula Yusnu <nunenuh@gmail.com>"

ENV PYTHON_VERSION=3.7
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/anaconda/bin:$PATH

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/anaconda && rm ~/anaconda.sh

RUN /opt/anaconda/bin/conda install -y python=$PYTHON_VERSION pytorch torchvision torchaudio cpuonly -c pytorch && \
    /opt/anaconda/bin/conda clean -tipsy && \
    ln -s /opt/anaconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh 

# set bashrc and activate conda
RUN echo ". /opt/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && \
	/opt/anaconda/bin/conda init bash && \
    echo "conda activate base" >> ~/.bashrc

# End Conda
# Tini: https://github.com/krallin/tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "--"]
# End Tini

RUN apt-get update && apt-get install -y build-essential libssl-dev
RUN pip install cmake

RUN pip install onnx onnxruntime tensorflow pytorch-lightning==1.4.5 mlflow==1.20 \
    deepface==0.0.68 deskew==0.10.33 dlib==19.22.1 face-recognition==1.3.0 face-recognition-models==0.3.0 \
    fire flask fonttools graphene graphql-core graphql-relay gunicorn \
    tensorboard==2.6.0 tensorboardx==2.4 tqdm==4.62.2 transformers==4.1.1 sqlalchemy spacy seqeval shapely \
    scikit-learn scikit-image retina-face==0.0.5 psycopg2==2.9.1 opencv-python==4.4.0.46 opencv-python-headless==4.5.1.48 \
    pandas  numpy multidict mtcnn==0.1.1 keras==2.6.0 keras-preprocessing==1.1.2 databricks-cli==0.15.0 \
    albumentations==1.0.3  alembic==1.4.1 augmentor==0.2.8 

#COPY ./req.txt /req.txt
#RUN chmod +x /req.txt
#RUN pip install -r req.txt --no-cache-dir

RUN apt update && apt install software-properties-common -y
RUN add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main" && apt update \
    && apt install libjasper1 libjasper-dev

RUN apt update && apt install -y cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev \
    python-dev python-numpy libtbb2 \
    libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev \
    libatlas-base-dev libjasper-dev python3-pyqt5


COPY ./start.sh /start.sh
RUN chmod +x /start.sh

CMD [ "/start.sh" ]


