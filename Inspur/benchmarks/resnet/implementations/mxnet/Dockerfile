# Copyright (c) 2018-2021, NVIDIA CORPORATION. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ARG FROM_IMAGE_NAME=nvcr.io/nvidia/mxnet:21.04-py3
FROM ${FROM_IMAGE_NAME}

# Install dependencies for system configuration logger
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
        infiniband-diags \
        pciutils \
 && rm -rf /var/lib/apt/lists/*

# Install Python dependencies
WORKDIR /workspace/image_classification

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy ResNet-50 code
COPY . .

# Configure environment variables
ENV MXNET_UPDATE_ON_KVSTORE=0      \
    OMPI_MCA_coll_hcoll_enable=0 \
    HCOLL_ENABLE_MCAST=0           \
    MXNET_EXEC_ENABLE_ADDTO=1      \
    MXNET_ASYNC_GPU_ENGINE=1       \
    HOROVOD_ENABLE_ASYNC_COMPLETION=1 \
    MXNET_USE_TENSORRT=0           \
    MXNET_GPU_WORKER_NTHREADS=1    \
    MXNET_GPU_COPY_NTHREADS=1      \
    MXNET_CUDNN_AUTOTUNE_DEFAULT=0 \
    HOROVOD_BATCH_D2D_MEMCOPIES=1  \
    HOROVOD_GROUPED_ALLREDUCES=1   \
    OMPI_MCA_btl=^openib \
    OMP_NUM_THREADS=1 \
    OPENCV_FOR_THREADS_NUM=1
