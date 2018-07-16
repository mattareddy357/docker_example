FROM ubuntu:14.04
RUN apt-get update && apt-get install -y software-properties-common supervisor 
RUN echo postfix postfix/main_mailer_type string "'Internet Site'" | debconf-set-selections && \
   echo postfix postfix/mailname string postfix | debconf-set-selections && \
   echo postfix postfix/mynetworks string "127.0.0.0/8" | debconf-set-selections && \
   apt-get --yes install postfix mailutils ca-certificates libsasl2-modules
RUN apt-get install -y vim mailutils

ENV gmail basivireddytestmail@gmail.com
ENV gmailpasswd cello143
RUN echo "[smtp.gmail.com]:587    $gmail:$gmailpasswd" > /etc/postfix/sasl_passwd 
RUN chmod 600 /etc/postfix/sasl_passwd
RUN postconf -e relayhost="[smtp.gmail.com]:587" && \
	postconf -e smtp_use_tls="yes" && \
	postconf -e mydestination="localhost.localdomain, localhost" && \
	postconf -e smtpd_banner='$myhostname ESMTP $mail_name' && \
	postconf -e smtp_sasl_auth_enable="yes" && \
	postconf -e smtp_sasl_security_options="" && \
	postconf -e smtp_sasl_password_maps="hash:/etc/postfix/sasl_passwd" && \
	postconf -e smtp_tls_CAfile="/etc/ssl/certs/ca-certificates.crt" && \
        postconf -e myhostname="postfix" && \
        postconf -e inet_protocols="ipv4" && \
        postconf -e mydestination=""

RUN postmap /etc/postfix/sasl_passwd
COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf
EXPOSE 25
#CMD /usr/bin/supervisord -c /etc/supervisor/conf.d/supervisord.conf
#CMD ["/usr/sbin/postfix","-c","/etc/postfix","start"]
CMD "/usr/bin/supervisord"
