FROM orion-ld-base
RUN apt-get -y install unzip python3-pip libffi-dev libcurl4-openssl-dev python3.7-dev libssl-dev debhelper
WORKDIR /opt
RUN git clone --branch v4.4 https://github.com/mongodb/mongo.git
WORKDIR /opt/mongo
RUN python3 -m pip install -r etc/pip/compile-requirements.txt
RUN python3 buildscripts/scons.py install-all --disable-warnings-as-errors MONGO_VERSION=4.4.0 CFLAGS="-march=armv8-a+crc -mtune=generic"
WORKDIR /opt/mongo/build
RUN mkdir mongodb-linux-4.4-arm64
RUN mv install/* mongodb-linux-4.4-arm64/
RUN strip mongodb-linux-4.4-arm64/bin/mongo
RUN strip mongodb-linux-4.4-arm64/bin/mongos
RUN strip mongodb-linux-4.4-arm64/bin/mongod
RUN cp ../src/mongo/installer/compass/install_compass mongodb-linux-4.4-arm64/bin
RUN tar -czf ../mongod-org-linux-4.4.0-arm64.tgz mongodb-linux-4.4-arm64
WORKDIR /opt/mongo/buildscripts
RUN python3 packager.py -a arm64 -d debian10 -s 4.4 -t /opt/mongo/mongo-4.4/mongod-org-linux-4.4.0-arm64.tgz --metadata-gitspec v4.4
