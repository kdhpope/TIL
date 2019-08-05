# Hadoop

왜 하둡인가.

하둡은 하드웨어에 관계없이 리눅스 기반으로 실행된다(PC에 들어간다.)(싸다!!)

(유닉스에선 오히려 안돔)

hadoop vs DB

DBMS가 처리하지 않는 부분을 hadoop이 한다. 대체재가 아닌 보완재임

무결성, 트랜잭션 처리를 하기 위해서는 RDBMS가 있어야 한다.

하둡은 큰 데이터를 저장하고, 처리만 함



#### NoSQL

sql을 안쓰는 DB(mongo DB등등, 데이터를 JSON으로 저장한다.)

기존의 DB는 중앙집중형식이라서 여러대의 DB를 사용할순 없다.(분산처리가 안된다는 말)

NoSQL은 기존 RDBMS처럼 완벽한 데이터 무결성과 정합성을 제공하지 않는다. 

핵심 데이터는 RDBMS를 사용하고 핵심이 아니지만 데이터를 보관하고 처리해야 하는 경우에 NoSQL을 이용한다.



공개키 암호화 방식

ssh

ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa

개인키와 공개키를 만든다.

cat id_dsa.pub >> authorized_keys(키를 배포? 한다?)



방화벽 해제

systemctl disable firewalld



### 설치

1. 필요 파일
   1. java(필수)
   2. hadoop
   3. eclipse
   4. tomcat
   5. mysql
2. hadoop.tar.zp 다운로드 및 압축해제
3. 호스트 네임 설정
   1. /etc/hosts
   2. /etc/hostname
4. systemctl disable firewalld(포트 넘버 때문에)
5. ip 고정
6. etc/profile에 hadoop path 추가
7. ssh
   1. 서버끼리 연결(공개키 암호화 방식)
8. 설정 변경
   1. conf/hdfs-site
   2. conf/core-site.xml
   3. conf/mapred-site.xml
9. 포맷
   1. hadoop namenode -format
10. 실행
    1. start-all.sh
    2. stop-all.sh(절대 그냥 끄지 마라)





### 명령어

대부분은 리눅스와 비슷함

형식 : hadoop dfs -(명령어)

ex) hadoop dfs -mkdir /test

