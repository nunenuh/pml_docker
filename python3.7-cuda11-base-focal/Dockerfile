FROM nunenuh/pml:cuda11.1-base-ubuntu20.04

LABEL maintainer="Lalu Erfandi Maula Yusnu <nunenuh@gmail.com>"

ENV PYTHON_VERSION=3.7
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/anaconda/bin:$PATH

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/anaconda && rm ~/anaconda.sh

RUN /opt/anaconda/bin/conda install -y python=$PYTHON_VERSION pytorch torchvision torchaudio \
    cudatoolkit=11.1 -c pytorch-lts -c nvidia && \
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


# RUN /opt/anaconda/bin/conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch-lts -c nvidia --y
# RUN /opt/anaconda3/bin/pip install pytorch-lightning==1.4.5 mlflow==1.20 \
#     tensorboard==2.6.0 tensorflow-gpu==2.6.0 torchmetrics==0.5.1 transformers==4.1.1 \
#     scikit-learn==0.23.2 spacy==2.3.2 spacy-lookups-data==0.3.2 nltk==3.5 \
#     opencv-python==4.4.0.46 opencv-python-headless==4.5.1.48 imutils==0.5.3 \
#     Augmentor==0.2.8 imgaug==0.4.0 albumentations==0.5.2 deskew==0.10.3 pytesseract \
#     deepface==0.0.68 face-recognition==1.3.0 face-recognition-models==0.3.0 mtcnn==0.1.1 

# RUN /opt/anaconda3/bin/pip install SQLAlchemy PyMySQL==0.10.1 psycopg2==2.9.1 
# RUN /opt/anaconda3/bin/pip install graphene==2.1.9 graphql-core==2.3.2 graphql-relay==2.0.1
# RUN /opt/anaconda3/bin/pip install seqeval==0.0.12 docker==5.0.0  
# RUN /opt/anaconda3/bin/pip install fire==0.3.1 fonttools==4.26.2 Shapely==1.7.1 \
#     terminado==0.9.3 Twisted==20.3.0 typing-extensions

RUN apt-get update && apt-get install -y build-essential libssl-dev
RUN pip install cmake

COPY ./conda.yml /conda.yml
RUN chmod +x /conda.yml
RUN /opt/anaconda/bin/conda env update -f conda.yml --prune 


COPY ./start.sh /start.sh
RUN chmod +x /start.sh

CMD [ "/start.sh" ]


