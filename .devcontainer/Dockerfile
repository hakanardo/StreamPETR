#FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-devel as build
FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-devel as build
RUN pip install vi3o

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
RUN apt-get update && apt-get --yes install git libglib2.0-0

RUN nvidia-smi
RUN pip install mmcv-full==1.6.0
RUN pip install flash-attn==0.2.2
RUN pip install scipy==1.7.3
RUN pip install mmdet==2.28.2
RUN pip install mmsegmentation==0.30.0
RUN pip install mmdet3d==1.0.0rc6

# --mount=type=cache,target=/root/.cache

# RUN cd ./StreamPETR
# RUN git clone https://github.com/open-mmlab/mmdetection3d.git
# RUN cd mmdetection3d
# RUN git checkout v1.0.0rc6
# RUN pip install -e .

# FROM pytorch/pytorch:1.9.0-cuda11.1-cudnn8-runtime as base
# COPY --from=build /opt/conda/lib/python3.10/site-packages/* /opt/conda/lib/python3.10/site-packages/
FROM build as base

FROM base as devel
RUN conda install --yes -c conda-forge pylint pyglet fontconfig
RUN apt-get update && apt-get --yes install libx11-6 libgl1 libglu1 fonts-freefont-ttf sudo

ARG USER=user
ARG UID=1000
ARG GID=1000

RUN groupadd -g $GID $USER
RUN useradd -m -u $UID -g $GID -s /bin/bash $USER
RUN echo $USER ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USER && chmod 0440 /etc/sudoers.d/$USER

USER $USER

