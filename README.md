# Banking-System-Blockchain-using-fabric
Demo đồ án IE105. Hệ thống ngân hàng dựa vào Blockchain system sử dụng fabric

# Install HyperLedger Fabric 1.4.1 and Composer 0.20.8 from Scratch on Ubuntu 18.04LTS
- B1: docker ps
- B2: sudo usermod -aG docker $USER 
- B3: su - <tên desktop> #restart the terminal 
- B4: # install docker-compose
   - sudo apt install docker-compose 
- B5: # kiểm tra version của docker-compose 
  - docker-compose --version
- B6: # install build essential for make, gcc, g++, (needed for nodejs later)
  - sudo apt install build-essential -y
- B7: # do an update and upgrade
  - sudo apt-get update
- B8: sudo apt-get -y upgrade
- B9: cd /tmp
- B10: # install go version 1.11.x required
  wget https://dl.google.com/go/go1.11.linux-amd64.tar.gz
- B11: sudo tar -xvf go1.11.linux-amd64.tar.gz
- B12: sudo mv go /usr/local 
- B13: # set the pathing for go and update ~/.profile
  - export GOROOT=/usr/local/go
  - export GOPATH=$HOME/go
  - export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
- B14: vi ~/.profile 
  - :set nu
  # thêm những dòng sau vào ~/.profile
  - GOROOT=/usr/local/go
  - GOPATH=$HOME/go
  - PATH=$GOPATH/bin:$GOROOT/bin:$PATH
  # lưu file :wq!
- B15: su - <tên desktop> #restart the terminal to prove ~/.profile works
- B16: source ~/.profile
- B17: # kiểm tra version go
  - go version
- B18: # install nodejs and npm with nvm. The other methods require sudo which cause file permission problems later.
  - curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
- B19: su - <tên desktop>
- B20: # nvm ls-remote will show you the version of node you can install. We want v8.17.0 (Latest LTS: Carbon)
  - nvm ls-remote 
  - nvm install v8.17.
- B21: # install version 5.6.0 of npm (required for HLF)
  - npm install npm@5.6.0 -g
- B22: Already install with sudo apt-get build-essential 
  - sudo apt install gcc g++ make 
- B23: # check the environment variables
  - env
  
######################### Hyperledger Fabric ################################
- B1: curl -sSL http://bit.ly/2ysbOFE | bash -s -- 1.4.1 1.2.1 0.4.15 #kéo repo fabric + download docker composer image.
- B2: vi ~/.profile 
# insert nvm vào
  - NVM_DIR=$HOME/.nvm
  - NVM_BIN=$NVM_DIR/version/node/v8.17.0/bin
  - PATH=$NVM_BIN:$PATH
  # lưu file và thoát :wq!
- B3: source ~/.profile 
- B4: 
# HLF 1.4 is now installed. We will next install Composer v0.20.8
  - npm install -g composer-cli@0.20
- B5: npm install -g composer-rest-server@0.20
- B6: npm install -g generator-hyperledger-composer@0.20
- B7: npm install -g yo
- B8: npm install -g composer-playground@0.20 
- B9: cd fabric-samples/basic-network
- B10: # start the basic network
  - ./start.sh
  - ./generate.sh 
- B11: # fabric is started - now start Composer
  - composer-playground 
===> Chạy thành công port:8000
=====================================================================
# Blockchain Based Banking System
- sudo apt install git
- Ubuntu 16.04 hoặc 18.04. điều đầu tiên bạn phải làm là cài đặt các yêu cầu trước cho dự án này. Làm điều đó bằng cách chạy tập lệnh **./prereqs-ubuntu.sh**
## Installing the Banking system using blockchain.
- pull the repository for yourself.
  - https://github.com/BluzOtter/Banking-System-Blockchain-using-fabric.git
  
  - cd Banking-System-Blockchain-using-fabric/fabric/first-network 
  
- chạy các lệnh này để chạy cài đặt blockchain và mã chaincode của nó. Chạy từng cái một.
  - ./byfn.sh up -s couchdb
  
  - docker exec cli peer chaincode install -n banking -l golang -p github.com/chaincode/banking -v 4.0
  
  - export ORDERER_CA=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem 
  
  - docker exec cli peer chaincode instantiate -o orderer.example.com:7050 --cafile $ORDERER_CA -C mychannel -c '{"Args":[]}' -n banking -v 4.0 -P "OR('Org1MSP.member', 'Org2MSP.member')"
  
-chạy 3 lệnh này để tạo ba người dùng mới cho ngân hàng, doanh nghiệp và cá nhân.
  - docker exec cli peer chaincode invoke -o orderer.example.com:7050 --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n banking --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt -c '{"Args":["addUser","Bank","City","bank@gmail.com","bank", "1000000","bank","National Bank"]}'
  
  - docker exec cli peer chaincode invoke -o orderer.example.com:7050 --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n banking --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt -c '{"Args":["addUser","Individual","City","individual@gmail.com","individual", "1000","individual","Student"]}'
  
  - docker exec cli peer chaincode invoke -o orderer.example.com:7050 --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n banking --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt -c '{"Args":["addUser","Business","City","business@gmail.com","business", "1000","business","Business"]}'
  
-Không đóng thiết bị đầu cuối này và mở một thiết bị đầu cuối mới trong thư mục Node_api.
- chạy các lệnh này.
  - npm install
  - node enrollAdmin.js
  - node registerUser.js
  - node app.js

- Thao tác này sẽ khởi động máy chủ API cho trang web. Giữ cho hai thiết bị đầu cuối này hoạt động.
- Đi tới thư mục SRC và mở trang web Index.html. Đăng ký Và tài khoản người dùng mới và tận hưởng ứng dụng.
## Blockchain trống và bắt đầu lại từ đầu.
- To empty blockchain. Go to fabric/first_network in blockchain and run
  - ./byfn.sh down
- sau đó đi đến thư mục node_APIs và xóa hfkey hoặc một cái gì đó tương tự như nó. Khi hai bước này được thực hiện, hãy bắt đầu từ bước cài đặt blockchain trong tài liệu này. mà không cần git kéo repo một lần nữa.
  
  
  
