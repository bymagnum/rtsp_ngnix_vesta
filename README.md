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






