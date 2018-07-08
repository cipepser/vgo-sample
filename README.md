# vgo-sample

[和訳: A Tour of Versioned Go (vgo) (Go & Versioning, Part2)](https://qiita.com/nekketsuuu/items/589bc29f00b507492a96)の写経です。

## 環境

```sh
❯ go version
go version go1.10.3 darwin/amd64
```

## 写経

### vgoのインストール

```sh
❯ go get -u golang.org/x/vgo
```

### サンプルプログラム

```go
// hello.go
package main // import "github.com/you/hello"

import (
	"fmt"

	"rsc.io/quote" // この時点では、cannot find packageになるけどvgoが解決してくれる
)

func main() {
	fmt.Println(quote.Hello())
}
```

あとは何も書いていない`go.mod`も作る



## references
* [A Tour of Versioned Go (vgo) (Go & Versioning, Part 2)](https://research.swtch.com/vgo-tour)
* [和訳: A Tour of Versioned Go (vgo) (Go & Versioning, Part2)](https://qiita.com/nekketsuuu/items/589bc29f00b507492a96)