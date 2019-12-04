# 概要

初期設定でラクするための ansible-playbook

## playbook の詳細

忘れるのでざっとメモ

### 構成

めっちゃざっくりだけど↓の感じ。

- site.yml
    - 実行する処理を一元管理
- inventory_file
    - 実行対象一覧
- roles
    - 実行する処理の詳細
        - hostname
            - ホスト名の変更: ホスト名は inventory_file の内容を使用する
        - sshd
            - sshd_config の変更: ポート変更などなど、変更先ポートは delaults で定義
        - firewalld
            - firewall の変更: SSHのアクセス許可ルールを変更
        - yumupdate
            - パッケージ更新: パッケージ更新有無にかかわらず再起動する

### `inventory_file` について

`inventory_file` で定義する名前はIPアドレスでもオッケー
別途↓のように設定すると `conoha-gespenst-jaeger` と指定するだけで対象サーバへ接続できる。

```
 $ cat ~/.ssh/config
Host conoha-gespenst-jaeger
  Port 2222
  IdentityFile ~/.ssh/id_rsa
  Hostname X.X.X.X
  User root
```

### Tree

結構細かく Role を分けたんだけどもうちょいまとめてもいいんかなぁ。構成はユルユル、テキトーです。

```
 $ tree .
.
├── inventory_file
├── roles
│   ├── firewalld
│   │   └── tasks
│   │       └── main.yml
│   ├── hostname
│   │   ├── delaults
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   ├── sshd
│   │   ├── delaults
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   └── yumupdate
│       └── tasks
│           └── main.yml
└── site.yml
```

# 参照サイト

- [Ansible Documentation](https://docs.ansible.com/)
- [Ansibleまとめ](https://qiita.com/WisteriaWave/items/0e5dda7ddc13b22188c7)
- [Ansible でSSHのセキュリティ設定](https://blog.apar.jp/linux/5299/)
- [macで Ansibleを使ってパスワード認証方式で ssh接続する](https://qiita.com/msrks/items/e0de2931ef726448696d)
- [Ansible マジック変数の一覧と内容](https://qiita.com/h2suzuki/items/15609e0de4a2402803e9)