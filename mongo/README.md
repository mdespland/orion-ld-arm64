```
https://github.com/mongodb/mongo.git
git checkout -b v4.0
apt-get install python-pip
apt install libffi-dev
python3 -m pip install -r buildscripts/requirements.txt
pip3 install scons
python3 buildscripts/scons.py mongo
```
```
apt-get -y install unzip
cd /opt
mkdir mongo
cd mongo
wget https://github.com/mongodb/mongo/archive/refs/heads/v4.0.zip
unzip v4.0.zip
cd mongo-4.0

git clone --branch v4.4 https://github.com/mongodb/mongo.git
cd mongo
python2.7 -m pip install -r buildscripts/requirements.txt

cd  build/
mkdir mongodb-linux-4.4-arm64
mv install/* mongodb-linux-4.4-arm64/
strip mongodb-linux-4.4-arm64/bin/mongo
strip mongodb-linux-4.4-arm64/bin/mongos
strip mongodb-linux-4.4-arm64/bin/mongod
cp ../src/mongo/installer/compass/install_compass mongodb-linux-4.4-arm64/bin
tar -czf ../mongod-org-linux-4.4.0-arm64.tgz mongodb-linux-4.4-arm64
cd ../buildscripts/
apt-get install debhelper

python3 packager.py -a arm64 -d debian10 -s 4.4 -t /opt/mongo/mongo-4.4/mongod-org-linux-4.4.0-arm64.tgz --metadata-gitspec v4.4

python3 buildscripts/scons.py install-all --disable-warnings-as-errors MONGO_VERSION=4.4.0 CFLAGS="-march=armv8-a+crc -mtune=generic"


#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wstringop-truncation"
    strncpy(d, s, 32);
#pragma GCC diagnostic pop

https://stackoverflow.com/questions/53094979/how-to-enable-wiringpi-gpio-control-inside-a-docker-container 