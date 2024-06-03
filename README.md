社内で利用する開発環境を作成しました。  
今後、適宜改変していく予定です。

---

### コンテナ

- PHP8.3
- Apache
- MySQL：最新最新版
- phpMyAdmin：最新安定版
- mailpit：最新安定版

---

### フォルダ構成
.
├── db
│   └── my.cnf
├── web
│   ├── html
│   │   ├── default
│   │   │   └── dist
│   │   │       └── index.php
│   ├── php
│   │   └── php.ini
│   └── org-apache-config.conf
├── docker-compose.yml
└── README.md

**注**：
主に永続化の為に使用する隠しフォルダ、隠しファイルは記載していません。

---

### 使い方
クローンして起動するだけで基本的には使えます。

1. クローンする
2. `docker compose up -d`

- WEB：http://localhost
- phpMyAdmin：http://localhost:8080
- Mailpit：http://localhost:8025

**注**：
apple silicon搭載PC用に作成しています。その他環境で使用する場合は**docker-compose.yml** 42行目のプラットフォーム指定を削除してください。


---

### 設定
ログイン情報等はdocker-compose.ymlで確認できます。

#### 初期設定
**MySQL**
- HOST：db
- USER：user
- PASS：password
- DATABASE：default

**MySQL（ROOT）**
- USER：root
- PASS：rootpassword

設定は適宜変更可能です。変更後は再ビルドしてください。

---

### マルチサイト
Apacheにマルチサイト設定が行えます。  
web > org-apache-config.conf にマルチサイトの設定を追記して再ビルドしてください。  
フォーマットはorg-apache-config.conf内にコメントアウトしています。  
ドメインを追加した際は併せてhostsファイルの編集もお忘れなく。

---

### 再ビルド

1. `docker compose down` (起動している場合)
2. `docker compose up --build -d`

---

### Linuxへのアクセス

`docker compose exec web bash`

でWEBコンテナに入れます。出る時は`exit`で出れます。

---

### WEBコンテナ内で出来る事

- composer
- nodenv

がプリインストールされています。モジュールが足りなく実行エラーが出る場合がありますがその際は追加するのでご一報ください。  
/var/www/html/フォルダ以下は永続化されていますがその他のフォルダは上記プリインストールツール以外は永続化されず、dockerを止めたら消えてしまうのでご注ください。