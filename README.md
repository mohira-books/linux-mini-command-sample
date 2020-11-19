# コマンドをつくりながら「パスを通す」を理解する
## `my name is bob` と出力する スクリプトを書こう
```sh
# 話を単純にするためにDesktopディレクトリで作業することにしましょう
$ cd ~/Desktop
/Users/bob/Desktop
```

```sh
# bob.shをつくります
$ echo "echo 'my name is bob'" > bob.sh

# bob.shの中身を確認します
$ cat bob.sh
echo 'my name is bob'
```

```sh
# まるで `python hello.py` のように実行できる
$ zsh bob.sh
my name is bob
```

## いちいち `zsh bob.sh` って面倒だなあ
`pwd` や `ls`  などのコマンド同じように `bob.sh` も実行できたらちょっとうれしいですよね。それを目指します。


```sh
# bob.sh をコマンドとして実行できるようにします
$ chmod +x bob.sh

# 実行権限がついたことを確認する
$ ls -l
total 8
-rwxr-xr-x  1 bob  member  22 11 19 19:08 bob.sh
```

```sh
# bob.sh を コマンドとして実行する(まるで pwd や ls と同じよう！)
$ ./bob.sh
my name is bob
```

```sh
# 当然フルパスでも実行可能
$ /Users/bob/Desktop/bob.sh
my name is bob
```

## `bob.sh`のパスを正確に指定しなければうまくいかない問題
```sh
# ホームディレクトリに移動する
$ cd ~
$ pwd
/Users/bob/Desktop

# ホームディレクトリ直下に bob.sh は存在しないので、当然エラー
$ ./bob.sh
zsh: no such file or directory: ./bob.sh
```

## `bob.sh` を 別の場所でも使えたら便利じゃないか？
現状を整理します。

- `bob.sh` は `my name is bob` と出力するスクリプトである
- `bob.sh` は `~/Desktop` に存在している
- `bob.sh` は コマンドとして実行が可能である
- `bob.sh` のパスを正確に指定しないとコマンドとして動かない

さて、`pwd` や `ls` なんかは、**どこでも使えますよね**？ これは非常に便利です。`bob.sh`も同様に**どこでも使える**ようにしましょう。

どうすれば良いと思いますか？


## どこでも使えるように、コマンドの検索パスに追加する

```sh
$ export PATH="$PATH:/Users/mohira/Desktop/hashiguchi"
```

```sh
# 一時的にPATHに Desktop を追加する
# シェルに再ログインしたり別途シェルを起動する場合はこの影響がないことに注意
$ export PATH="$PATH:/Users/bob/Desktop"
```

```sh
# デスクトップじゃないディレクトリにいることを確認する
$ pwd
/Users/bob

# デスクトップ以外のディレクトリでも bob.sh コマンドが使えている！
$ bob.sh
my name is bob


# which で確認すると 確かに デスクトップの bob.sh をみていることがわかる
$ which bob.sh
/Users/bob/Desktop/bob.sh
```


## まとめ
- 「パスを通す」の「パス」とはなにか
  - 環境変数の `PATH` を指す
  - この文脈においては、ファイル「パス」という意味の「パス」ではない
- パスを通す
  - ある特定のコマンドを任意のディレクトリから実行するために行うもの
  - 具体的には `PATH`という環境変数に情報を追加することを指す
    - コマンドを実行するときは、`PATH`という環境変数の情報使ってコマンドを探しに行くため

## より詳しくは参考書を読みながら手を動かしましょう！
[新しいLinuxの教科書](https://www.amazon.co.jp/dp/4797380942)をやればもっと理解は深まるし、めっちゃ楽しいと思います！



以上
