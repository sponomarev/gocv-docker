FROM golang:1.10-alpine

LABEL maintainer="Sergey Ponomarev <sponomarev@evilmartians.com>"
LABEL description="This image contains all the dependencies to compile and run GoCV project"

ENV OPENCV_VERSION=3.4.1
ENV TMP_DIR="/tmp/opencv"

ENV BUILD="build-base \
    linux-headers \
    libjpeg-turbo \
    jasper-libs \
    tiff \
    libwebp \
    libpng \
    libgphoto2"

ENV BUILD_DEPENDENCIES="cmake \
    libjpeg-turbo-dev \
    jasper-dev \
    tiff-dev \
    libwebp-dev \
    libpng-dev \
    libgphoto2-dev"

RUN apk --no-cache add ${BUILD}
RUN apk --no-cache add --virtual .build-dep ${BUILD_DEPENDENCIES}

RUN mkdir ${TMP_DIR} && \
    cd ${TMP_DIR} && \
    wget -O opencv.zip https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.zip && \
    unzip opencv.zip && \
    wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.zip && \
    unzip opencv_contrib.zip && \
    mkdir ${TMP_DIR}/opencv-3.4.1/build && cd ${TMP_DIR}/opencv-${OPENCV_VERSION}/build && \
    cmake \
    -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D OPENCV_EXTRA_MODULES_PATH=${TMP_DIR}/opencv_contrib-${OPENCV_VERSION}/modules \
    -D WITH_FFMPEG=NO \
    -D WITH_IPP=NO \
    -D WITH_OPENEXR=NO \
    -D WITH_TBB=YES \
    -D INSTALL_C_EXAMPLES=NO \
    -D INSTALL_PYTHON_EXAMPLES=NO \
    -D BUILD_ANDROID_EXAMPLES=NO \
    -D BUILD_DOCS=NO \
    -D BUILD_TESTS=NO \
    -D BUILD_PERF_TESTS=NO \
    -D BUILD_EXAMPLES=NO \
    -D BUILD_opencv_java=NO \
    -D BUILD_opencv_python2=NO \
    -D BUILD_opencv_python3=NO .. && \
    make -j4 && \
    make install && \
    cd && rm -rf ${TMP_DIR}

RUN apk del .build-dep

ENV PKG_CONFIG_PATH /usr/local/lib64/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib64
ENV CGO_CPPFLAGS -I/usr/local/include
ENV CGO_CXXFLAGS "--std=c++1z"
ENV CGO_LDFLAGS "-L/usr/local/lib -lopencv_core -lopencv_face -lopencv_videoio -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_video -lopencv_dnn -lopencv_xfeatures2d -lopencv_plot -lopencv_tracking"