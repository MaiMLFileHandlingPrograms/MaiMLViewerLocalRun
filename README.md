# MaiMLViewerLocalRun
# ローカル環境でdockerを使わずにMaiMLViewerを開発＆実行する環境の構築

## 構成
#### A: xmail-viewer(アプリケーションサーバー)
    node.js
#### B: graph-db(DBサーバー)
    neo4j 4.4
#### C: DBにアクセスする一部を担うpythonスクリプト
    python3

</br>
</br>

## インストールするもの
#### A: xmail-viewer(アプリケーションサーバー)
      node.js, node-modules(package.json,package-lock.json)
#### B: graph-db(DBサーバー)
      openjdk 11, neo4j 4.4
#### C: DBにアクセスする一部を担うpythonスクリプト
      pythonの最新版, python Packages(neo4j==4.4,lxml==5.0.0,cryptography==3.3.2,signxml==2.10.1)

</br>
</br>

## 環境構築
### 準備: GitHubからコード一式をローカル環境にダウンロードする
    例として、ディレクトリを"/user/local/MaiMLViewerLocalRun/"とする
***
### A: xmail-viewer(アプリケーションサーバー)
  #### A-1 nodeをインストール
    　公式サイト(https://nodejs.org/)からダウンロードしてインストール
    　nodeとnpmがインストールされる
  #### A-2 関連モジュールをインストール
    　"/user/local/MaiMLViewerLocalRun/xmail-viewer/"ディレクトリに、package.json、package-lock.jsonの２つのファイルが存在する
    　
     下記コマンドを実行
    　> cd /user/local/MaiMLViewerLocalRun/xmail-viewer/
    　> npm install
    　  →"/user/local/MaiMLViewerLocalRun/xmail-viewer/node_modules/"が作成される
  #### A-3　nodeを起動
    　下記コマンドを実行
	　> cd /user/local/MaiMLViewerLocalRun/xmail-viewer/
	　> node /bin/www
  #### A-4 webブラウザで "http://localhost:3000/" にアクセス
    　接続できていればエラーになっていてもOK
  #### A-5 nodeを停止
	　Ctl+c
***
### B: graph-db(DBサーバー) 
下記をそれぞれ自分の環境に合わせてインストール、実行する
  #### B-1 JDK 11をインストール
    　それぞれの環境に合わせて、openjdk11.x.xxをインストールし、２つの環境変数を追加
    　JAVA_HOME=インストールしたディレクトリ
    　PATH=$JAVA_HOME/bin:$PATH
  #### B-2 neo4j 4.4をインストール
    　公式サイトからneo4j 4.4-community版をダウンロード
    　ダウンロードしたneo4j-community-4.4.xx-unix.tar.gzを解凍し、
    　任意のディレクトリ（例えば、/user/local/neo4j/）におく
  #### B-3 neo4jを起動
    　下記コマンドを実行
	　> cd /user/local/neo4j/bin/
	　> ./neo4j console　もしくは　> ./neo4j start
		Starting Neo4j.
		Started neo4j (pid:14213). It is available at http://localhost:7474
  #### B-4　webブラウザで "http://localhost:7474" にアクセス
    　start画面が出たらOK
  #### B-5 neo4jを停止
    　下記コマンドを実行
	　> cd /user/local/neo4j/bin/
    　> ./neo4j stop
***
### C: DBにアクセスする一部を担うpythonスクリプト
    自分の環境に合わせてpython3をインストールする
    また、下記のパッケージをインストールする
    　neo4j==4.4、lxml==5.0.0、cryptography==3.3.2、signxml==2.10.1
</br>
</br>

## アプリケーションを実行する
### 1: 環境変数を設定する
    export NEO4J_HOME=/usr/local/neo4j
    export PATH=$PATH:$NEO4J_HOME/bin
    export MAIML_TMP_DIR=/user/local/MaiMLViewerLocalRun/xmail-viewer/models/tmp
### 2: neo4jの設定&起動
    neo4j.confの設定を変更する
    場所：/user/local/neo4j/conf/neo4j.conf
    修正内容：
        dbms.security.auth_enabled=false
        dbms.directories.data=/usr/local/neo4j/data
        dbms.directories.logs=/usr/local/neo4j/logs
        dbms.connector.bolt.enabled=true
        dbms.connector.bolt.listen_address=:7687
        dbms.connector.http.enabled=true
        dbms.connector.http.listen_address=:7474
    修正後neo4jを再起動：
        > neo4j restart
### 3: nodeの起動
    下記コマンドを実行
	　> cd /user/local/MaiMLViewerLocalRun/xmail-viewer/
	　> node /bin/www
        →"/user/local/MaiMLViewerLocalRun/xmail-viewer/logs"が作成される
### 4: webブラウザでアプリケーションにアクセス
    URL："http://localhost:3000/"にアクセス
    MaiMLViewerのリスト画面が表示される
