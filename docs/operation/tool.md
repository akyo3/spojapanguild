# SPO JAPAN GUILD TOOL

最終更新日：2022/12/07 v3.4.6

!!! info "主な機能"


## **設定**

**スクリプトダウンロード**
```
cd $NODE_HOME/scripts
wget https://raw.githubusercontent.com/btbf/spojapanguild/master/scripts/sjgtool.sh -O sjgtool.sh
chmod 755 sjgtool.sh
```

**envファイル修正**

そのまま全コピーして実行してください。
```
sed -i $NODE_HOME/scripts/env \
    -e '1,73s!#WALLET_PAY_ADDR_FILENAME="payment.addr"!WALLET_PAY_ADDR_FILENAME="payment.addr"!' \
    -e '1,73s!#WALLET_STAKE_ADDR_FILENAME="reward.addr"!WALLET_STAKE_ADDR_FILENAME="stake.addr"!' \
    -e '1,73s!#POOL_HOTKEY_VK_FILENAME="hot.vkey"!POOL_HOTKEY_VK_FILENAME="kes.vkey"!' \
    -e '1,73s!#POOL_HOTKEY_SK_FILENAME="hot.skey"!POOL_HOTKEY_SK_FILENAME="kes.skey"!' \
    -e '1,73s!#POOL_COLDKEY_VK_FILENAME="cold.vkey"!POOL_COLDKEY_VK_FILENAME="node.vkey"!' \
    -e '1,73s!#POOL_COLDKEY_SK_FILENAME="cold.skey"!POOL_COLDKEY_SK_FILENAME="node.skey"!' \
    -e '1,73s!#POOL_OPCERT_COUNTER_FILENAME="cold.counter"!POOL_OPCERT_COUNTER_FILENAME="node.counter"!' \
    -e '1,73s!#POOL_OPCERT_FILENAME="op.cert"!POOL_OPCERT_FILENAME="node.cert"!' \
    -e '1,73s!#POOL_VRF_SK_FILENAME="vrf.skey"!POOL_VRF_SK_FILENAME="vrf.skey"!'
```

=== "各ファイル名を独自のファイル名変更している場合"
    ~/cnode/scripts/envファイル内の以下の変数に対し、先頭の#を削除しご自身の環境に合わせてファイル名を設定してください。  
    !!! memo ""
        WALLET_PAY_ADDR_FILENAME="payment.addr"  
        WALLET_STAKE_ADDR_FILENAME="stake.addr"  
        POOL_HOTKEY_VK_FILENAME="kes.vkey"  
        POOL_OPCERT_FILENAME="node.cert"  
        POOL_VRF_SK_FILENAME="vrf.skey"  
        POOL_OPCERT_COUNTER_FILENAME="node.counter"  
        POOL_OPCERT_FILENAME="node.cert  
        POOL_VRF_SK_FILENAME="vrf.skey


**スクリプトへのパスを通し、任意の単語で起動出来るようにする**
```
echo alias gtool="'cd $NODE_HOME/scripts; ./sjgtool.sh'" >> $HOME/.bashrc
source $HOME/.bashrc
```

TOOLを実行する
```
gtool
```

!!! bug "既知の不具合"

    * **[2] ブロック生成状態チェック**

    1.対象プール、プールIDがnullになる場合があります。  
    2.エポック切り替わり後、半日～1日までは有効ステークが0ADAになります。  

    上記の不具合は、チェーンデータ取得用APIの既知の不具合によるもので、現在修正依頼中です。
    もしこの項目でNGが出た場合は、ディスコードでご質問ください。

    また、その他バグを発見した場合はGithubで[issue](https://github.com/btbf/spojapanguild/issues)を提出してください。

## 更新履歴

### 3.4.6

    * ブロック生成状態チェック
        * メタデータハッシュチェック追加
        * PeerInカウントから127.0.0.1を除外
    * PreviewネットワークTx確認リンクをCardanoScanへ変更

### 3.4.4

    * Preview / PreProd テストネット対応（envファイル更新必須）
    * ブロック生成チェック総合判定導入

### 3.4.3

    * 軽微な修正

### 3.4.2

    * 364特別対応削除
    * envファイルアップデートチェックフラグ切替機能追加
    * トランザクション送信後、トランザクション確認URLを表示

### 3.3.1

    * 364エポック特別対応 cncli.shパッチ適用追加

### 3.3.0

    * 報酬/資金出金　任意アドレス出金ADA Handleに対応

### 3.2.1

    * エアギャップコマンドを修正

### 3.2.0

    * KES更新時のカウンターファイルチェック・作成手順追加
    * ブロック生成状態チェック KESファイルチェックルール変更

### 3.1.0

    * KES更新時のnode.certバックアップ/削除追加

### 3.0.0

!!! hint ""

    * KES更新機能追加

    このアップデートに伴い、envファイルの修正が必要となります。
    v2.x.x以上をすでにインストールされていて、プールのKES秘密鍵のファイル名がkes.skeyの場合は以下のコードを1回だけ実行してください。
    違うファイル名をご利用の場合は任意に書き換えてください。
    ```
    sed -i $NODE_HOME/scripts/env \
    -e '1,73s!#POOL_HOTKEY_SK_FILENAME="hot.skey"!POOL_HOTKEY_SK_FILENAME="kes.skey"!'
    ```

### 2.1.0

!!! hint ""

    * DdzFアドレスへの報酬/資金出金を許可
    * payment.addr出金時、出金額表示単位バグを修正