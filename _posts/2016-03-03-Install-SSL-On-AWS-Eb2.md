---
layout: post
title: 무료로 아마존 서버에 SSL을 설치해보자.
---

무료로 SSL을 설치해보자.
wild card 서비스 같은경우엔 30만원/년 이나 한다.. 
그런데 이걸 공짜로 가능하다!

0. 요구사항
  AWS, EC2, Amazon OS, apache
  아마존 ec2 콘솔에서 포트 SSH 22, HTTP 80, HTTPS 443 inbound 오픈 (당연 중요!)

1. 인증파일을 얻어보자.
  letsencrypt에서 무료로 ssl인증을 받을 수 있다. 아직 아마존os는 자동 설치가 안되서, 인증서를 따로 받고 수동설치를 해야한다. 80번포트로 인증을 하기 때문에 기존의 httpd를 멈추고 받아야한다.
  [참고](https://letsencrypt.org/getting-started/)
```
  $ sudo service httpd stop
  $ git clone https://github.com/letsencrypt/letsencrypt
  $ cd letsencrypt
  $ ./letsencrypt-auto certonly
  $ sudo service httpd start
  $ sudo ls /etc/letsencrypt/archive/
```
2. 생성된 인증파일을 설치해 보자.
```
  $ sudo vim /etc/httpd/conf.d/ssl.conf 
```
  위에서 생성된 파일을 잘 보고 잘 넣는다.
```
  SSLCertificateFile  /etc/letsencrypt/archive/wp.homcle.com/cert1.pem
  SSLCertificateKeyFile /etc/letsencrypt/archive/wp.homcle.com/privkey1.pem
  SSLCertificateChainFile /etc/letsencrypt/archive/wp.homcle.com/chain1.pem
```
3. ssl을 활성화 시키자.
  [참고](http://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/SSL-on-an-instance.html)
```
  $ sudo yum update -y
  $ sudo yum install -y mod24_ssl
  $ sudo service httpd restart
```
4. 무료 ssl을 즐긴다!
  참고로 https사이트 에선 http의 그 어떤것도 부를 수가 없다.
  //yoururl.com 형식으로 작성하는 습관을

