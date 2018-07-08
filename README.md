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

### Upgrade

`-u`オプションでupdated packageを確認できる。元記事のときから変わって`[]`内にLATESTが表示されるようになったみたい。

```sh
❯ vgo list -u -m all
vgo: finding golang.org/x/text v0.3.0
vgo: finding rsc.io/sampler v1.99.99
github.com/you/hello
golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c [v0.3.0]
rsc.io/quote v1.5.2
rsc.io/sampler v1.3.0 [v1.99.99]
```

`golang.org/x/text`をUpgradeしてみる。

```sh
❯ vgo get golang.org/x/text
vgo: downloading golang.org/x/text v0.3.0
```

`go.mod`が変わっている。

```sh
❯ git diff go.mod
diff --git a/go.mod b/go.mod
index 3200210..6246735 100644
--- a/go.mod
+++ b/go.mod
@@ -1,3 +1,6 @@
 module github.com/you/hello

-require rsc.io/quote v1.5.2
+require (
+       golang.org/x/text v0.3.0
+       rsc.io/quote v1.5.2
+)
```

listで見ても以下のように`v0.0.0-20170915032832-14c0d48ead0c`から`v0.3.0`になっていることがわかる。

```sh
❯ vgo list -m all
github.com/you/hello
golang.org/x/text v0.3.0
rsc.io/quote v1.5.2
rsc.io/sampler v1.3.0
```



## references
* [A Tour of Versioned Go (vgo) (Go & Versioning, Part 2)](https://research.swtch.com/vgo-tour)
* [和訳: A Tour of Versioned Go (vgo) (Go & Versioning, Part2)](https://qiita.com/nekketsuuu/items/589bc29f00b507492a96)