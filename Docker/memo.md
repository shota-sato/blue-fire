# Docker起動手順

## Docker Desktopのインストール	
	https://www.docker.com/products/docker-desktop/  

## Docker HubからMySQLイメージを取得
`docker pull mysql:latest`  
Dockerイメージをリモートリポジトリ（通常はDocker Hub）からローカルにダウンロードするための基本的なコマンド  

## MySQLコンテナを起動
`docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<your_password> -d -p 3306:3306 mysql:latest`  
--name mysql-container: コンテナの名前を指定  
-e MYSQL_ROOT_PASSWORD=<your_password>: MySQLのrootユーザーのパスワードを環境変数として設定します。  
-d: コンテナをバックグラウンドで実行します。  
-p 3306:3306: ホストのポート3306をコンテナのポート3306にマッピングします。  

## コンテナが正常に起動しているか確認
`docker ps`  

## Dockerコンテナ内に入ってMySQLに接続
`docker exec -it mysql-container mysql -u root -p`  

## MySQL操作
`CREATE DATABASE my_database;`  
MySQLに接続したら、データベースを作成することができます  


