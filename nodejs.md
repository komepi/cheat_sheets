## バージョン管理(nvm)

バージョン切り替えで有名なのがnvm
### インストール手順(wsl)
以下はwinのwslでのインストール時に参考にした手順
参考: https://learn.microsoft.com/ja-jp/windows/dev-environment/javascript/nodejs-on-wsl

1. nvmをインストール
   ```
   $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
   ```
   確認には`command -v nvm`を入力。"nvm"が返される
   できない場合はターミナルを閉じてから改めてターミナルを開くとできるようになる
2. 現在インストールされているバージョンの確認
    `nvm ls`で確認できる
    最初は以下のような画面のはず(何もない)
    ```
    ->     system
    iojs -> N/A (default)
    node -> stable (-> N/A) (default)
    unstable -> N/A (default)
    ```
3. node.jsのインストール
    安定版と最新版がある
    * 安定版
        `nvm install --lts`
    * 最新版
        `nvm install node`

### インストール可能なバージョン一覧
`nvm ls-remote`
利用かのなNode.jsのLTSバージョンのみの表示
`nvm ls-remote --lts`

### インストール済みのバージョン一覧
`nvm ls`
```
$ nvm ls
->       v8.16.1
        v10.16.3
          system
default -> v10.16.3
```
バージョンリストとsystemがnvmで管理されているnode.jsのバージョン
systemはnvmを使わないでインストールしたバージョン
`->`でポイントされているのが現在使用中のバージョン

### インストール
`nvm install`でインストールできる
`nvm install v10.16.3`のようにinstallの次にバージョンを指定すると、指定したバージョンをインストールできる

### バージョンの切り替え
`nvm user {version}`で切り替え
```
$ nvm ls
         v8.16.1
   ->   v10.16.3
          system
$ nvm user v8.16.1
$ nvm ls
->       v8.16.1
        v10.16.3
          system
```