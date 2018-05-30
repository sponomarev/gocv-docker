FROM golang:1.10-alpine

LABEL maintainer="Sergey Ponomarev <sponomarev@evilmartians.com>"
LABEL description="This image contains all the dependencies to run already compiled GoCV project"

ENV BUILD="build-base \
    linux-headers \
    libjpeg-turbo \
    jasper-libs \
    tiff \
    libwebp \
    libpng \
    libgphoto2"

RUN apk --no-cache add ${BUILD}

ENV PKG_CONFIG_PATH /usr/local/lib64/pkgconfig
ENV LD_LIBRARY_PATH /usr/local/lib64
ENV CGO_CPPFLAGS -I/usr/local/include
ENV CGO_CXXFLAGS "--std=c++1z"
ENV CGO_LDFLAGS "-L/usr/local/lib -lopencv_core -lopencv_face -lopencv_videoio -lopencv_imgproc -lopencv_highgui -lopencv_imgcodecs -lopencv_objdetect -lopencv_features2d -lopencv_video -lopencv_dnn -lopencv_xfeatures2d -lopencv_plot -lopencv_tracking"