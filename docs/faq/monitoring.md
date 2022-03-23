# ノード監視に関するよくある質問

## Q1.Grafanaのログインパスワードを忘れました

??? note "A.ログインに必要な情報を初期化します"
    リレー１にて以下を実施します。
    ```
    sudo sqlite3 /var/lib/grafana/grafana.db
    ```
    ```
    update user set password = '59acf18b94d7eb0694c61e60ce44c110c7a683ac6a8f09580d626f90f4a242000746579358d77dd9e570e83fa24faa88a8a6', salt = 'F3FAxVm33R' where login = 'admin';
    ```
    ```
    .quit
    ```
    以下の情報でログインし、パスワードを再設定してください

    Username:`admin`  
    password:`admin`


