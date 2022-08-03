# gclog
場所は`/{elasticsearchのインストール場所}/logs/gc.log

# 設定ファイル
jvmのヒープサイズを変更するには`/{elasticsearchのインストール場所}/config/jvm.options`を編集する必要がある。
このファイルの中でも、以下の個所を編集する
```
################################################################
## IMPORTANT: JVM heap size
################################################################
##
## You should always set the min and max JVM heap
## size to the same value. For example, to set
## the heap to 4 GB, set:
##
## -Xms4g
## -Xmx4g
##
## See https://www.elastic.co/guide/en/elasticsearch/reference/current/heap-size.html
## for more information
##
################################################################

# Xms represents the initial size of total heap space
# Xmx represents the maximum size of total heap space

-Xms1g
-Xmx1g
```
だいたいファイルの先頭あたりにある。最後２行を変更する。1gbなら`-Xms1g`など、4gbなら`-Xms4g`など、200MBなら`-Xms200m`など