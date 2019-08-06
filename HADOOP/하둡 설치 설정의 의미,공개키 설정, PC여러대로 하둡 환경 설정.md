## 설정의 의미



name node - 사용자에게 다른 노드들을 추상화 해 준다? 다른 노드들과 사용자를 연결시켜줌

sub name node - namenode의 설정을 '저장'만 해줌( namenode를 대신할순 없다)

conf/core-site.xml

##### hdfs-site.xml

replication : 파일을 몇개로 복사할 것인가

dfs.name.dir : name node directory

dfs.data.dir : data node directory

dfs.webhdfs.enabled : 웹으로 접근 가능할 것인가

##### mapred-site.xml

job tracker : 분석처리를 명령 받아서 task tracker가 실행되게 함

task tracker의 결과를 리턴해줌



hadoop 여러개 만들기

## 공개키 설정

- 이 설정을 해주는 이유는 서버간 통신시 비밀번호를 매번 입력해야 하기 때문에 키 설정을 해서 해당 서버에서 접속할 때는 비밀번호 입력을 안하게 끔 하는 것이다.

```shell
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa // 비밀키 생성
cd .ssh // ssh 디렉터리에 키 생성되므로 생성되었는지 확인
cat id_dsa.pub >> authorized_keys // 비밀키에 맞는 공개키 생성

//ssh root@hadoop2 cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
//ssh root@hadoop3 cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
//ssh root@hadoop4 cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys

// 위 방법 안되서 scp 사용해서 전송
scp authorized_keys root@hadoop2:~/.ssh/authorized_keys
scp authorized_keys root@hadoop3:~/.ssh/authorized_keys
scp authorized_keys root@hadoop4:~/.ssh/authorized_keys

//각 서버에 공개키 뿌려줌

// 각 서버에 authorized_keys 생성되었는지 확인 후
// hadoop1에서 ssh 실행해서 비밀번호 하는지 안하는지 확인

ssh hadoop2
ssh hadoop3
ssh hadoop4
// 만일 비밀번호를 입력하라고 나오면 설정이 잘못된 것
// .ssh 디렉터리 다 지우고 다시해야 한다.
```



Agent admitted failure to sign using the key. 오류가 뜨면

ssh-add 해주면 해결됨



## PC 여러대로 하둡 환경 설정

1. ssh 활성화(ssh-keygen)

2. 설정 파일 변경

   1. core-site.xml

      fs.dafault.name : namenode의 ip:port

   2. mapred-site.xml

      mapred.job.tracker : jobtracker의 ip:port

   3. hdfs-site.xml

      replication : 데이터의 복사를 몇개나 할 것인가

      http.address : 하둡의 아이피를 어디로 둘 것인가.

   4. slaves 

      데이터를 저장하고 관리할 곳(데이터 노드) 다른 형식 없이 IP나 도메인 네임만 기술

      ```xmk
      hadoop2
      hadoop3
      hadoop4
      ```

   5. master

      보조 네임노드를 실행할 서버

3. systemctl stop firewalld

   systemctl disable firewalld

4. namenode 컴퓨터에서 hadoop 설치 및 configureation

   * \- /etc/profile
   * \- /etc/bashrc

5. hadoop.tar.gz 각 서버에 복사

6. namenode format

7. start-all.sh



### 구성 (예시)



> hadoop1
>
> > JobTracker
>
> > NameNode



> hadoop2
>
> > second NameNode
>
> > DataNode
>
> > Task Tracker



> hadoop3
>
> > DataNode
>
> > Task Tracker



> hadoop4
>
> > DataNode
>
> > Task Tracker



