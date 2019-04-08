# iRedMail Docker Container #

iRedMail allows to deploy an OPEN SOURCE, FULLY FLEDGED, FULL-FEATURED mail server in several minutes, for free. If several minutes is long time then this docker container can reduce help you and deploy your mail server in seconds.

Current version of container uses MySQL for accounts saving. Container contains all components (Postfix, Dovecot, Fail2ban, ClamAV, Roundcube, and SoGo) and MySQL server. The hostname of the mail server can be set using the normal docker methods (```docker run -h <host>``` or setting 'hostname' in a docker compose file). In order to customize the container several environmental variables are allowed:
  * MYSQL_ROOT_PASSWORD - Root password for MySQL server installation
  * POSTMASTER_PASSWORD - Initial password for postmaster@DOMAIN. Password can be generated according to [wiki](http://www.iredmail.org/docs/reset.user.password.html). ({PLAIN}<password>)
  * TZ - Container timezone that is propagated to other components
  * SOGO_WORKERS - Number of SOGo workers which can affect SOGo interface performance.

Container is prepared to handle data as persistent using mounted folders for data.
Folders prepared for initialization are:
  * /var/lib/mysql
  * /var/vmail
  * /var/lib/clamav

Build:
```
docker build iredmail-docker --tag ired:latest
```

Run:
```
docker run --privileged -p 80:80 -p 443:443 \
           -h <hostname.domain> \
           -e "MYSQL_ROOT_PASSWORD=<password>" \
           -e "SOGO_WORKERS=1" \
           -e "TZ=Europe/Prague" \
           -e "POSTMASTER_PASSWORD={PLAIN}<password>" \
           -e "IREDAPD_PLUGINS=['reject_null_sender', 'reject_sender_login_mismatch', 'greylisting', 'throttle', 'amavisd_wblist', 'sql_alias_access_policy']" \
           -v PATH/mysql:/var/lib/mysql \
           -v PATH/vmail:/var/vmail \
           -v PATH/clamav:/var/lib/clamav \
           --name=iredmail ired:latest
```
 
 



