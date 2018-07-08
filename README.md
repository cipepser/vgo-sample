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

### vgo build

```sh
❯ vgo build
vgo: resolving import "rsc.io/quote"
vgo: finding rsc.io/quote v1.5.2
vgo: finding rsc.io/quote (latest)
vgo: adding rsc.io/quote v1.5.2
vgo: finding rsc.io/sampler v1.3.0
vgo: finding golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c
vgo: downloading rsc.io/quote v1.5.2
vgo: downloading rsc.io/sampler v1.3.0
vgo: downloading golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c
```

ちゃんとbuildされる

```sh
❯ ./hello
こんにちは世界。
```

空だった`go.mod`にも追記されている

```sh
// go.mod
module github.com/you/hello

require rsc.io/quote v1.5.2
```

依存しているモジュールを`vgo list -m`で表示できるっぽいけど、1個しか出てこない？
→`vgo list -m all`で全部出てくる。

```sh
❯ vgo list -m all
github.com/you/hello
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c
rsc.io/quote v1.5.2
rsc.io/sampler v1.3.0
```



## references
* [A Tour of Versioned Go (vgo) (Go & Versioning, Part 2)](https://research.swtch.com/vgo-tour)
* [和訳: A Tour of Versioned Go (vgo) (Go & Versioning, Part2)](https://qiita.com/nekketsuuu/items/589bc29f00b507492a96)