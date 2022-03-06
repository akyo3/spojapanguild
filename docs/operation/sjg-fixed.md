# SJG 修正プログラム

## **envファイル対応**

!!! info ""
    最終更新日：2022/02/19 v1.0


**概要**

**■発生事象**  
gliveview.sh/cncli.sh実行時に、実行したディレクトリにenv/gliveview.sh/cncli.shが生成されてしまう。

**■原因**  
env内のアップデート関数処理変更に伴うもの

**■修正プログラム内容**  
⇒.bashrc内aliasに設定中の変数(glive/blocks)のコマンドを変更  
⇒cncli系サービスファイルのWorkingDirectoryを変更

**■対象者**
* プール構築中でブロックログの設定まで完了した方
* プール運営中SPOの方

**■対象サーバー**  
BP/リレー  

### **■修正プログラム適用手順**

#### BP／リレー共通

**1.修正プログラムをダウンロードする(BP／リレー共通)**
```
cd $NODE_HOME/scripts
wget -q https://raw.githubusercontent.com/btbf/coincashew/master/guild-tools/service_fixed.sh -O service_fixed.sh
```

**2.パーミッションを設定し、実行する**
```
chmod 755 service_fixed.sh
./service_fixed.sh
```

**3.プログラムが終了するのを待つ**

**4. bashrcを読み込む**
```
source $HOME/.bashrc
```


**5.修正プログラムを削除する**
```
cd $NODE_HOME/scripts
rm service_fixed.sh
```

**6.アップデート確認対応**

```
cd $HOME
glive
```
> Script update(s) detected, do you want to download the latest version? (yes/no):***_yes_*** 

**再度、gliveviewを実行する**
```
cd $HOME
glive
```
アップデート確認が出なくなるまで**_yes_** で対応する。


#### BPのみ


**9.（BPのみ）各サービスが動いているか確認する**
!!! info ""
    複数のサービスでアップデート確認が表示されている場合がありますが、どれか１つで`yes`にしたら他のサービスでは`no`にしてください  
    例）  
    * tmux a -t cncliでは`yes`  
    `Script update(s) detected, do you want to download the latest version? (yes/no):yes`  
    　  
    * tmux a -t validateでは`no`  
    `Script update(s) detected, do you want to download the latest version? (yes/no):no`


```
tmux a -t cncli
```
```
tmux a -t validate
```
```
tmux a -t leaderlog
```
```
tmux a -t logmonitor
```

!!! info ""
tmux a -t leaderlogは、今回のプログラムでは中断処理を入れてるので以下の表記になっていますが問題ありません。

    > Checking for script updates...  
    > ^C  

    次エポックのスケジュールを取得したい場合は、別途ノードを再起動してください。



お疲れさまでした。


## 作業ディレクトリ名修正

最終更新日：2021年12月27日の時点　version 2.2

!!! info "概要"
    * ノードを止めるため、ブロック生成予定が無い時間帯に実行してください。
    * BP/リレーは自動判定し、それぞれの依存ファイルを修正します。  
    * 環境変数$NODE_HOMEに設定されているパスを読み取り自動置換します。  
    ex) /home/user/cardano-my-node ⇒ /home/user/cnode  
    ex) /home/user/cardano-node ⇒ /home/user/cnode
    * 現在設定中のノードポート番号やプールIDを読み取り、各ファイルに自動反映します。


!!! danger "注意"
    🛑 **日本語マニュアルで使われてる各種スクリプトのファイル名を任意の名前に変更している場合は、正しく動作しない場合があります。** 🚧  
    もし、スクリプトファイルやキーファイル名を変更している場合は実行前にBTBFまでご相談ください。


### 1. プログラムをダウンロードする

```bash
cd $HOME
wget -q https://raw.githubusercontent.com/btbf/coincashew/master/guild-tools/dirname_cng.sh -O dirname_cng.sh
```

### 2.プログラムを実行する

```bash
chmod 755 dirname_cng.sh
./dirname_cng.sh
```

> 画面の指示に従って進めてください。

### 3.起動確認


ノードログ表示
```
journalctl --unit=cardano-node --follow
```
> エラーが出てる場合は、$HOME直下にある「dirname-＊＊＊.log」ログファイルをギルドに提出してください。

環境変数確認
```
source $HOME/.bashrc
echo $NODE_HOME
```
> 例) /home/user/cnode　と表示されればOK 


gliveview確認
> 表示できるか確認してください。
> Script update(s) detected, do you want to download the latest version? (yes/no):」が表示される場合は、出なくなるまで `Y`

ノード別確認事項

=== "リレーノード1"

    clontab確認
    ```
    crontab -l
    ```
    >　例） "22 * * * * /home/user/cnode/topologyUpdater.sh" のように表示されればOK  



=== "ブロックプロデューサーノード"

    ブロックログ起動確認
    ```
    tmux a -t cncli
    ```
    > ～～～100.00% syncedが表示されたらOK  
    > 赤文字はノードが同期するまで表示される文章であり、エラーではないのでノードが同期するまでお待ちください

    アップデート警告文がある場合
    > Script update(s) detected, do you want to download the latest version? (yes/no):」が表示されている場合は `Y`
    デタッチしてからサービス再起動
    ```
    sudo systemctl reload-or-restart cnode-cncli-sync.service
    ```

    再度確認
    ```
    tmux a -t cncli
    ```
    > ～～～100.00% syncedが表示されたらOK  


実行ファイル削除

```
rm $HOME/dirname_cng.sh
```

### 4.エアギャップディレクトリ名変更
!!! info ""
    エアギャップでは、上記のプログラムを実行する必要はありません。


```
cd $HOME
mv ./cardano-my-node ./cnode
```

```
sed -e "s@cardano-my-node@cnode@g" -i $HOME/.bashrc
source $HOME/.bashrc
echo $NODE_HOME
```
> 例) /home/user/cnode　と表示されればOK 