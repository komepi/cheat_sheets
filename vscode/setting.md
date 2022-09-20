# setting.json
## ターミナル
### 表示行: terminal.integrated.scrollback
`terminal.integrated.scrollback`によってターミナルの行バッファの最大値を設定できる
デフォルトでは1000

## workspace上の監視の無視：files.watchExclude
ワークスペースにファイル数が増えると、`Visual Studio Code is unable to watch for file changes in this large workspace`といったような注意が発生することがある。
これがあると、例えばmarkdown preview enhanceが正常に動作しないなどの現象が起こる可能性がある。
その対処の一つとして、`.venv`のような大量のディレクトリやファイルを含むけど実際に使うことがないディレクトリを監視から外すこと
以下のように監視対象から外すディレクトリを指定することができる。
```json
{
  "file.watchExclude":{
    "**/.git/**": true,
    "**/.venv/**": true,
    "**/build/**": true
  }