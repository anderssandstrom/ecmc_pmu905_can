git clone https://github.com/linux-can/can-utils
cd can-utils
make
sudo make install


git clone git https://github.com/CANopenNode/CANopenSocket.git
cd CANopenSocket
git submodule init
git submodule update
cd tools
./get_tools.sh


cd CANopenNode
make

update gcc if needed (if error at __has_include):
sudo yum install centos-release-scl
sudo yum install devtoolset-7-gcc*
scl enable devtoolset-7 bash
which gcc
gcc --version



Follow this:
https://github.com/CANopenNode/CANopenNode/blob/master/doc/gettingStarted.md
sudo modprobe vcan
sudo ip link add dev vcan0 type vcan
sudo ip link set up vcan0


update 