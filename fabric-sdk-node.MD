Exercise fabric sdk on LinuxOne VM
# https://github.com/hyperledger/fabric-sdk-node

# prepare tool
# go
mkdir -p /data/setup
cd /data/setup
wget https://storage.googleapis.com/golang/go1.8.linux-s390x.tar.gz
    

export GOPATH=/go
mkdir -p $GOPATH/src/github.com/hyperledger/
cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric-ca.git

cd $GOPATH/src/github.com/hyperledger/fabric-ca
make docker-clean
make docker

cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric.git

cd $GOPATH/src/github.com/hyperledger/fabric
make docker-clean
make docker

cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric-sdk-node.git
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-node

cd $GOPATH/src/github.com/hyperledger/fabric-sdk-node/test/fixtures
### Do below change for file docker-compose.yaml
### replace with your couchdb image name, get it with below command
### docker images |grep fabric-couchdb
189c189
<     image: couchdb
---
>     image: hyperledger/fabric-couchdb:s390x-1.0.0-snapshot-5b59e06 

docker-compose up --force-recreate
or:
docker-compose up -d



## Test
rm -rf /tmp/hfc-*
rm -rf ~/.hfc-key-store
gulp test

docker run -d -p 5984:5984 --name my-couchdb hyperledger/fabric-couchdb:latest

curl localhost:5984/



### BACKUP
### install couch DB
cd $GOPATH/src/
wget http://www-eu.apache.org/dist/couchdb/source/2.0.0/apache-couchdb-2.0.0.tar.gz
tar -zxf apache-couchdb-2.0.0.tar.gz
cd $GOPATH/src/apache-couchdb-2.0.0
Change below line for file Dockerfile
FROM debian:jessie -> FROM s390x/debian:jessie
docker build $PWD
