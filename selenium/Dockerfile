FROM openjdk:8-jre

# SOAP UI Version to download
ENV SOAPUI_VERSION 5.5.0

# Download and unarchive SoapUI
RUN mkdir -p /opt &&\
    curl  https://s3.amazonaws.com/downloads.eviware/soapuios/${SOAPUI_VERSION}/SoapUI-${SOAPUI_VERSION}-linux-bin.tar.gz \
    | gunzip -c - | tar -xf - -C /opt && \
    ln -s /opt/SoapUI-${SOAPUI_VERSION} /opt/SoapUI

# Place add jar files to SoapUI classpath
COPY /ext /opt/SoapUI/bin/ext

# Remove old guava version
RUN rm -v /opt/SoapUI/lib/guava-14.0.jar
# Replace it with new guava version
COPY /lib /opt/SoapUI/lib

# Update apk repo
RUN echo "http://dl-4.alpinelinux.org/alpine/v3.10/main" >> /etc/apk/repositories && \
    echo "http://dl-4.alpinelinux.org/alpine/v3.10/community" >> /etc/apk/repositories

# Install chromedriver
RUN apk update
RUN apk add chromium chromium-chromedriver


# Set display port to avoid crash
ENV DISPLAY=:99

# Set environment
ENV PATH ${PATH}:/opt/SoapUI/bin

WORKDIR /opt/SoapUI/bin
ENTRYPOINT ["testrunner.sh", "PPATH_TO_DRIVER=/usr/local/bin"]