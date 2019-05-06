# rtsp/rtmp + nginx + Vesta Control Panel + CentOS 7

1. Инсталируем ОС.

2. Инсталируем Vesta Panel без nginx
<pre>
curl -O http://vestacp.com/pub/vst-install.sh

bash vst-install.sh --nginx no --apache yes --phpfpm no --named yes --remi yes --vsftpd yes --proftpd no --iptables yes --fail2ban yes --quota no --exim yes --dovecot yes --spamassassin no --clamav yes --softaculous yes --mysql yes --postgresql no --hostname localhost.ltd --email exemple@domain.ru --force
</pre>

3. Устанавливаем через SSH: <br>
<pre>
yum -y groupinstall 'Development Tools'
yum -y install libxml2-devel libxslt-devel gd-devel perl-ExtUtils-Embed GeoIP-devel
yum install nano -y
yum install vim -y
yum install hg -y
yum install cmake -y
yum install gcc gcc+ -y
yum install openssl openssl-devel -y
yum install php-devel -y
yum install git -y
yum install libxslt-devel -y
yum install pcre-devel -y
yum install libpcre3 libpcre3-dev libssl-dev -y
yum install rtmpdump -y
yum update -y
</pre>

4. Переходим к сборке и компилированию nginx + rtmp 
скачиваем nginx:
<pre>
wget http://nginx.org/download/nginx-1.10.2.tar.gz
</pre>

распаковываем:

<pre>
tar -xzvf nginx-1.10.2.tar.gz
</pre>

скачиваем nginx-rtmp-module (https://github.com/arut/nginx-rtmp-module):

<pre>
wget https://github.com/arut/nginx-rtmp-module/zipball/master -O nginx-rtmp-module-master.zip
</pre>

распаковываем:
<pre>
unzip nginx-rtmp-module-master.zip -d nginx-rtmp-module-master
</pre>

переходим в папку распакованного nginx-1.10.2:

<pre>
cd nginx-1.10.2
</pre>

собираем

(необходимо изменить arut-nginx-rtmp-module-43f1e42 - последние значения на свои):
<pre>
./configure --prefix=/usr --add-module=../nginx-rtmp-module-master/arut-nginx-rtmp-module-43f1e42/ --pid-path=/var/run/nginx.pid --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-http_ssl_module
</pre>

если ошибок нет, идем дальше:
<pre>
make
make install
</pre>
переходим в root папку:
<pre>
cd
</pre>

5. Устанавливаем ffmpeg.

Удаляем файлы repo atrpms (offline)
<pre>
/etc/yum.repos.d
</pre>
Далее:
<pre>
yum repolist - посмотреть список (уточняем есть ли в списке atrpms)
yum clean all - очистить кеш 
yum makecache - создать кеш
</pre>

Создаем repo: 
<pre>
yum install http://repo.okay.com.mx/centos/6/x86_64/release/okay-release-1-1.noarch.rpm
</pre>
Или руками пишем любым редактором: 
<pre>
[okay]
name=Extra OKay Packages for Enterprise Linux - $basearch
baseurl=http://repo.okay.com.mx/centos/$releasever/$basearch/release
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-OKAY

[okay-debuginfo]
name=Extra OKay Packages for Enterprise Linux - $basearch - Debug
baseurl=http://repo.okay.com.mx/centos/$releasever/$basearch/debug
failovermethod=priority
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-OKAY
gpgcheck=1
</pre>

Далее:
<pre>
yum install ffmpeg
</pre>
Если ранее производились какие либо действия по установке ffmpeg - то возвращаемся к очистке yum и созданию нового кеша yum.

6. Копируем stat.xsl (обращаем также внимание на 791b613)
<pre>
cp /root/nginx-rtmp-module-master/arut-nginx-rtmp-module-791b613/stat.xsl /etc/nginx
</pre>
