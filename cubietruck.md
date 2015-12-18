##cubietruck安装

###common soft

sudo apt-get install aptitude tmux unrar-free unzip curl nethogs htop socat -y
sudo apt-get install git subversion mercurial build-essential make -y
#sudo apt-get install libav-tools #avconv

###mjpeg-streamer

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


###ffmepg

sudo apt-get install yasm -y
sudo aptitude install libx264-dev libfaac-dev libmp3lame-dev libtheora-dev libvorbis-dev libxvidcore-dev libxext-dev libxfixes-dev -y
wget http://ffmpeg.org/releases/ffmpeg-2.8.3.tar.bz2
tar -xjvf ffmpeg-2.4.1.tar.bz2  

./configure --prefix=/usr/local/ffmpeg --enable-gpl --enable-version3 --enable-nonfree --enable-postproc --enable-pthreads --enable-libfaac --enable-libmp3lame --enable-libtheora --enable-libx264 --enable-libxvid --enable-x11grab --enable-libvorbis  
make  
make install  


###nginx rtmp

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

or
wget http://nginx.org/download/nginx-1.9.X.tar.gz  x为最新版本号，自行更换 2015.12.18为1.9.9
tar zxvf nginx-1.9.X.tar.gz


sudo apt-get install gstreamer1.0-tools gstreamer1.0-plugins-good screen -y

