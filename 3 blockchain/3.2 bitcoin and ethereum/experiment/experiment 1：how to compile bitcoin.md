# Ubuntu编译和部署bitcoin

## 依赖

`sudo apt-get update`

`sudo apt-get upgrade`

`sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev`

`sudo apt-get install libboost-all-dev`

`sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev protobuf-compiler`

`sudo apt-get install libqrencode-dev`

`sudo apt-get install libminiupnpc-dev`

## 源码

1. 安装git

`sudo apt install git`

2. 下载bitcoin源码

`git clone https://github.com/bitcoin/bitcoin.git`

## 安装bitcoin

1. 创建目录
`cd bitcoin`
`mkdir db4/`

2. berkeley-db
`wget http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz`
`tar -xzvf db-4.8.30.NC.tar.gz`
`cd db-4.8.30.NC/build_unix/`
`../dist/configure --enable-cxx --disable-shared --with-pic --prefix=/root /bitcoin/db4/`
`make`
`sudo make install`
3. 安装bitcoin
`cd ~/bitcoin/`
`./autogen.sh`
`./configure LDFLAGS="-L/root/bitcoin/db4/lib/" CPPFLAGS="-I/root/bitcoin/db4/include/"`
`make`
`sudo make install`
## 运行bitcoin
`bitcoind`