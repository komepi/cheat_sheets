# launch.jsonの書き方
参考：[【VSCode】launch.jsonについて理解する | amateur engineer's blog](https://amateur-engineer-blog.com/vscode-launchjson/#toc4)
デバッグを行う際、`.vscode/launch.json`にある構成をもとにデバッグが行われる。
例えば以下のようなもの(コメントは削除している)
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Current File",
      "type": "python",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    }
  ]
}
```
`version`や`configurations`の部分は自動で作成される。自分で編集するのは`configurations`以下の辞書型部分。
主な要素とその意味は以下の通り
 * **name**: デバッグ実行の名前
 * **type**: 使用するDebugger
 * **request**: デバッグ実行のモード(launchかattach)
 * **program**: デバッグ対象のプログラムのパス
 * **console**: デバッグの結果を出力するコンソール
 * **cwd**: 現在の作業ディレクトリ
 * **args**: デバッグ実行時に渡される引数

その他にも要素はある
（参考：[Debugging in Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging#_launchjson-attributes))

またこの中で使用できる定義済みの変数とその意味は以下の通り
 * **${file}**: 現在開いているファイルのパス
 * **${fileBasename}**: 現在開いているファイル名
 * **${workspaceFolder}**: VSCodeで開いているフォルダのパス
 * **{workspaceFolderBasename}**: VSCodeで開いているフォルダ名
 * **${cwd}**: 現在の作業ディレクトリ
参考:[Visual Studio Code Variables Reference](https://code.visualstudio.com/docs/editor/variables-reference)