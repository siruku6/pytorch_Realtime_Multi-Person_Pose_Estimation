FROM python:3.10-slim

RUN apt -y update && apt -y upgrade
RUN apt -y install \
    # Necessary for pip install
    gcc g++ libc-dev \
    # For psycopg2 (postgresql client)
    libpq-dev \
    # For development and debug
    bash vim \
    # For scikit-learn (scipy)
    gfortran libopenblas-dev liblapack-dev \
    # For opencv-python
    libopencv-dev \
    # For building pafprocess module
    swig \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# for updating python module
RUN pip install --upgrade pip \
    && pip install pipenv

EXPOSE 8000
ARG WORKDIR="opt"
WORKDIR /${WORKDIR}

# Install pip modules
ARG LOCAL_DOCKERFILE_ROOT
COPY ${LOCAL_DOCKERFILE_ROOT}/requirements.txt ./
RUN pip install -r requirements.txt

# Compile the C++ postprocessing (for `pytorch_Realtime_Multi-Person_Pose_Estimation`)
COPY lib/pafprocess/ lib/pafprocess/
RUN cd lib/pafprocess/ && sh make.sh
