# Go mod コードをパッケージにわける

## プロジェクトに名前をつける

自分のプロジェクトに名前をつける。
なんでもよい。が、世の中で一意になる名前にすると都合が良い。
よって、github.com/username/project_name のようにするのが最も無難。
(※ username、project_name は実際のものに置き換えること。)

## go mod init ...

コマンドライン上でイニシャライズする。

```sh
go mod init "github.com/username/project_name"
```

## コードの分離(パッケージの分離)

main パッケージの main 関数が入っている main.go から書きはじめたとする。
この main.go が長くなってきた＋機能別にパッケージを分けたい、となった。分離したい機能を foo とする。

foo を分離する前の main.go はこのような内容。

```go
package main

import "fmt"

func Foo() {
	fmt.Println("Foo!")
}

func main() {
	Foo()
}
```

main.go がある階層にサブディレクトリ foo を作り、その中に foo.go を置く。

ファイルの位置関係はこのようになる。

```txt
./
|- main.go
|- foo
   |- foo.go
```

上記の foo/foo.go の内容はこう。

```go
package foo

import "fmt"

func Foo() {
	fmt.Println("Foo!")
}
```


foo.go は package foo とし、Foo という関数をエクスポートしている。
(※ 関数の先頭が大文字になっていることに注目。)

これを main.go 内で foo.Foo として使いたい。
この場合、main.go 内の import ブロックの中に `github.com/username/project_name/foo` を含めれば良い。

```go
package main

import (
	"github.com/username/project_name/foo"
)

func main() {
	foo.Foo()
}
```

こうしておいて、main.go のある階層で go build すればコンパイルされる。

```sh
go build
```

## 参考

Go Wiki Modules > Quick start https://github.com/golang/go/wiki/Modules#quick-start


ローカル環境のみでの Go Modules と自作パッケージ https://qiita.com/tkj06/items/a5f79417935100045650

## 関連記事

Go mod コードをパッケージにわける https://note.com/kokoronopython/n/necc0eea74035
