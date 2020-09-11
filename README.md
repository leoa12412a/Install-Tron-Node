# Install-Tron-Node 安裝波場節點

## 用Java安裝

1.安裝Java參考<a href="https://www.liquidweb.com/kb/install-java-8-on-centos-7/">這裡</a>跟<a href="https://stackoverflow.com/questions/45182717/java-home-is-set-to-an-invalid-directory">這裡</a>
```
yum -y update
yum install java-1.8.0-openjdk
java -version
```

2.設定Jave-home
  
  找到java路徑
  ```
  update-alternatives --config java
  ```
  
  輸出為
  ```
  [root@124-219-96-45 java-tron]# update-alternatives --config java

  共有 1 个提供“java”的程序。

    选项    命令
  -----------------------------------------------
  *+ 1           java-1.8.0-openjdk.x86_64 (/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64/jre/bin/java)
  ```
  
3.設定Java-home路徑 路徑取到bin之前
```
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64/jre
```

4.開始安裝波場節點 首先間建立專案資料夾
```
mkdir -p  /project/tron/
```

5.下载java tron
```
cd   /project/tron/
git clone -b master https://github.com/tronprotocol/java-tron.git
```

6.編譯java-tron項目
```
cd ./java-tron 
./gradlew build
```

7.啟動節點 

  全節點
  ```
  nohup java -Xmx6g -XX:+HeapDumpOnOutOfMemoryError -jar FullNode.jar  -c main_net_config.conf
  ```
  
  超級節點
  ```
  nohup java  -XX:+UseConcMarkSweepGC -jar FullNode.jar  -p  private key --witness -c main_net_config.conf
  ```

  如果輸出錯誤如下
  ```
  nohup: 忽略输入并把输出追加到"nohup.out"
  ```
  
  解決方法參考<a href="http://www.yayihouse.com/yayishuwu/chapter/1656">這裡</a>
  ```
  nohup java -jar do_iptable.jar >/dev/null 2>&1 &
  ```

## 用Docker安裝
安裝內容參考至<a href="https://github.com/tronprotocol/java-tron">官方github</a>

1.安裝docker
```
yum install docker -y
```

2.安裝git
```
yum install git
```

3.使用git下載java-tron，並進入java-tron(如果要使用官方image可以直接跳4-2)
```
git clone https://github.com/tronprotocol/java-tron.git
cd java-tron
```

4.建立或拉取docker image

  4-1.建立docker image
  ```
  docker build -t tronprotocol/java-tron .
  ```
  4-2.拉取官方docker image
  ```
  docker pull tronprotocol/java-tron
  ```

5.運行docker容器
```
docker run -it -d -p 8090:8090 -p 8091:8091 -p 18888:18888 -p 50051:50051 --restart always tronprotocol/java-tron 
```

## 使用docker-tron-quickstart

1.安裝Node.js
```
yum install nodejs
```

2.git下載TRON Quickstart
```
git clone https://github.com/TRON-US/docker-tron-quickstart.git
```

3.拉取quickstart image
```
docker pull trontools/quickstart
```

4.執行容器
--rm選項退出後會自動刪除容器。這非常重要，因為容器無法重新啟動，必須從頭開始運行它才能正確配置環境。
```
docker run -it \
  -p 9090:9090 \
  --rm \
  --name tron \
  trontools/quickstart
```

如果一切順利，您的終端控制台輸出將如下所示：
```
Private Keys
==================

(0) 9b811e3bd9ea5f4614279f0b4641dc592ab3942719261ec4af169e15c143aa63
(1) 909f6c7cdbd793239b869891dbfead24d247fc831042d7ac89a524cd85964d11
(2) 7174081512d6af7778e8eea06477d1d503e3d03b25b53a110c2ad3dfc52a33d7
(3) bdda4da1d4dcf6bb52402c95b2b5d29adc861e8b18a4384588384c3fd3b3a228
(4) 9a48973816628326474a947f79519fdb95263700d4a8f93a44f50b005c0454d0
(5) f64282de60a01b8b769f85ffed760d7d0f5bae6819968f7341aa2b24359b6771
(6) 05077c1e17e06053920f3ff56819f370f374bd3f4c802056c7922b2983d46bce
(7) 5ab38680cfebc7c915c87e1cf3b7f347769dc2dd4eb7e0e8fee234dccb83ce2f
(8) b3c29a199809155d57c975ea485cb98f058eda6bfd105e342969a3ef967b34ea
(9) 7ac0d9e9ed02930dac43f8c2f5c621c9c4aadae880c8fe822e803117d6c5ebcc

HD Wallet
==================
Mnemonic:      silly giant broom trim wink hill sphere any leg diesel math skate
Base HD Path:  m/44'/195'/0'/0/{account_index}

GET 200  - 22389.461 ms
```
出現GET 200後就可以去瀏覽器上打開http://127.0.0.1:9090/
將會看到一段json的數據
```
{"Welcome to":"TronGrid v3.4.2","API version":"v1"}
```
