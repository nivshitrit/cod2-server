Init code:

sudo dpkg --add-architecture i386
sudo apt update
sudo yum install -y git libc6:i386 libgcc-s1:i386 wget bzip2 unzip
git clone https://github.com/nivshitrit/cod2-server.git
wget http://icculus.org/updates/cod/gcc3-libs.tar.bz2
tar -xvjf gcc3-libs.tar.bz2
cp libstdc++.so.5 libgcc_s.so.1 /root/cod2-server/call-of-duty-2-docker-server/cod2server/

1) Download zpam (place in cod2server/main)
https://github.com/eyza-cod2/zpam3

2) Download & copy game files to moshe's server to cod2server/main (iw_0x.iwd & localized_english_iw0x.iwd files)
Game files download: https://www.mediafire.com/file/eaaswgygabdzmgd/Call+of+duty+2.rar/file

Zip the iwd files and then copy to server:
scp -P 23 PATH/cod2.zip root@149.106.140.65:/root/cod2-server/call-of-duty-2-docker-server/cod2server/main

3) Run server with this command
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. LD_PRELOAD=libCoD2x.so ./cod2_lnxded +set dedicated 2 +set net_port 28961 +exec server.cfg

