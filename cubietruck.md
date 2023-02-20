
# cubietruck使用笔记
![Markdown](https://github.com/forrestsun/cubieboard/blob/master/img/cubietruck.jpg)
## 硬件特性：
1. AllWinnerTech SOC A20，ARM® Cortex™-A7 Dual-Core，ARM® Mali400 MP2 Complies with OpenGL ES 2.0/1.1
2. 1GB/2GB DDR3@480MHz (960MTPS)
3. HDMI&VGA 1080P display output on-board
4. 10M/100M/1G Ethernet
5. Wifi+BT wireless connection with antenna on-board
6. SATA 2.0 interface support 2.5’ HDD，(for 3.5’ HDD, only need another 12V power input)
7. Storage solution：NAND+MicroSD or TSD+ MicroSD or 2*MicroSD
8. 2 x USB HOST，1 x OTG，1 x Toslink (SPDIF Optical)，1 x IR，4 x LEDs，1 Headphone，3 x Keys
9. Power：DC5V @ 2.5A with HDD，support Li-battery & RTC
10. 54 extended pins including I2S, I2C, SPI, CVBS, LRADC x2,UART, PS2, PWMx2, TS/CSI, IRDA, LINEIN&FMIN&MICIN, TVINx4 with 2.0 pitch connectors
11. PCB size：11cm *8cm*1.4mm，very suite for installing a 2.5’ HDD


## 常用软件安装
---
```bash
sudo apt-get install aptitude tmux unrar-free unzip curl nethogs htop socat -y
sudo apt-get install git subversion mercurial build-essential make -y
sudo apt-get install libav-tools #avconv
```

## 视频处理
### mjpeg-streamer安装
---
```bash
sudo apt-get install subversion  -y 
sudo apt-get install libv4l-dev libjpeg8-dev  -y
sudo apt-get install imagemagick fswebcam v4l-utils -y

sudo fswebcam -v
cd ~/
mkdir code
cd code
svn co https://svn.code.sf.net/p/mjpg-streamer/code/mjpg-streamer/
cd mjpg-streamer/mjpg-streamer
make USE_LIBV4L2=true clean all
sudo make DESTDIR=/usr install
```

### ffmep安装
---
```bash
sudo apt-get install yasm -y
sudo aptitude install libx264-dev libfaac-dev libmp3lame-dev libtheora-dev libvorbis-dev libxvidcore-dev libxext-dev libxfixes-dev -y
wget http://ffmpeg.org/releases/ffmpeg-2.8.3.tar.bz2
tar -xjvf ffmpeg-2.4.1.tar.bz2  

./configure --prefix=/usr/local/ffmpeg --enable-gpl --enable-version3 --enable-nonfree --enable-postproc --enable-pthreads --enable-libfaac --enable-libmp3lame --enable-libtheora --enable-libx264 --enable-libxvid --enable-x11grab --enable-libvorbis  
make  
make install  
```

### nginx rtmp安装
---
```bash
sudo apt-get update
sudo apt-get -y install nginx
sudo apt-get -y remove nginx
sudo apt-get clean

sudo rm -rf /etc/nginx/*
sudo apt-get install -y curl build-essential libpcre3-dev libpcre++-dev zlib1g-dev libcurl4-openssl-dev libssl-dev
mkdir -p ~/nginx_src
cd ~/nginx_src
git clone https://github.com/arut/nginx-rtmp-module.git
git clone https://github.com/nginx/nginx.git

cd nginx
./configure --prefix=/var/www --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --pid-path=/var/run/nginx.pid --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-http_ssl_module --without-http_proxy_module --add-module=/root/code/nginx_src/nginx-rtmp-module

make
make install
```
或者直接去下源码包，x为最新版本号，自行更换 （2015.12.18为1.9.9）
```bash
wget http://nginx.org/download/nginx-1.9.X.tar.gz  
tar zxvf nginx-1.9.X.tar.gz
```
```bash
sudo apt-get install gstreamer1.0-tools gstreamer1.0-plugins-good screen -y
```
### 测试
#### ffmpeg CPU占用率40%~60%/摄像头
```bash
./ffmpeg -r 30 -f video4linux2 -i /dev/video0 -vcodec libx264 -r:v 25 -b:v 800k -f mpeg1video -preset ultrafast -s 640x480 -f flv rtmp://192.168.8.140/rtmp/live
```
#### mjpeg CPU占用率2%~10%
```bash
mjpg_streamer -i "/usr/lib/input_uvc.so -d /dev/video0 -r 1280x720 -f 30" -o "/usr/lib/output_http.so  -w /usr/www"
```
