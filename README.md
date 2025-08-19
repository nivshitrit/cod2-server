If fresh clone, do only 1 & 2. To simply start the server just do 3.

1) Download zpam (place in cod2server/main)
https://github.com/eyza-cod2/zpam3

2) Copy game files to moshe's server to cod2server/main (iw_0x.iwd & localized_english_iw0x.iwd files)
scp -P 23 PATH/cod2.zip root@149.106.140.65:/root/call-of-duty-2-docker-server/cod2server/main

3) Run server with this command
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:. LD_PRELOAD=libCoD2x.so ./cod2_lnxded +set dedicated 2 +set net_port 28961 +exec server.cfg

