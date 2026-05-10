TODO: 再度見直し
TODO: 全て理解

---

# Go

## Hello World!

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, 世界")
}
```

## ローカルで Go を動かす

以下のサイトから Go のダウンロードとインストール。

https://go.dev/dl/

## Packages

https://go-tour-jp.appspot.com/basics/1

- Go のプログラムは、パッケージ( package )で構成される
- プログラムは main パッケージから開始される
- 規約で、パッケージ名はインポートパスの最後の要素と同じ名前になる
  - e.g. インポートパスが "math/rand" のパッケージは、 package rand ステートメントで始まるファイル群で構成する

```go
package main

// 括弧でパッケージのインポートをグループ化し、factoredインポートステートメント( factored import statement )としている
import (
	"fmt"
	"math/rand"
)
// 複数のインポートステートメントで書くこともできる↓
// import "fmt"
// import "math"

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```

## Exported names

https://go-tour-jp.appspot.com/basics/3

- Go では、最初の文字が大文字で始まる名前は、外部のパッケージから参照できるエクスポート（公開）された名前( exported name )
  - e.g. Pi は math パッケージでエクスポートされている
- 小文字ではじまる pi や hoge などはエクスポートされていない名前
- パッケージをインポートすると、そのパッケージがエクスポートしている名前を参照することができる
- エクスポートされていない名前(小文字ではじまる名前)は、外部のパッケージからアクセスすることはできない

```go
package main

import (
	"fmt"
	"math"
)

func main() {
  // 成功する
	fmt.Println(math.Pi)
  // 失敗する
	fmt.Println(math.pi)
}
```

## Functions

https://go-tour-jp.appspot.com/basics/4

- 関数は、0 個以上の引数を取ることができる
- この例では、 add 関数は、 int 型の２つのパラメータを取る
- 変数名の **後ろ** に型名を書くことに注意

```go
package main

import "fmt"

func add(x int, y int, z int) int {
	return x + y - z
}

func main() {
	fmt.Println(add(42, 13, 55))
	// 0
}
```

## Functions continued

- 関数の２つ以上の引数が同じ型である場合には、最後の型を残して省略して記述できる

```go
package main

import "fmt"

func add(x, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

## Multiple results

https://go-tour-jp.appspot.com/basics/6

- 関数は複数の戻り値を返すことができる
- 戻り値に名前をつけると、関数の最初で定義した変数名として扱われる

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
	return y, x
}

func main() {
	a, b := swap("hello", "world")
	fmt.Println(a, b)
}
```

## Named return values

https://go-tour-jp.appspot.com/basics/7

- 戻り値に名前を付けることができる
- 名前をつけた戻り値の変数を使うと、 return ステートメントに何も書かずに戻すことができる
- これを "naked" return と呼ぶ
- naked return ステートメントは、短い関数でのみ利用すべき
- 長い関数で使うと読みやすさ( readability )に悪影響がある

```go
package main

import "fmt"

func minus(sum int) (x, y int) {
	x = sum - 1
	y = sum - 2
	return
}

func main() {
	fmt.Println(minus(3))
}
```

## Variables

https://go-tour-jp.appspot.com/basics/8

- var ステートメントは変数( variable )を宣言する
- 関数の引数リストと同様に、複数の変数の最後に型を書くことで、変数のリストを宣言できる
- var ステートメントはパッケージ、または、関数で利用できる

```go
package main

import "fmt"

var c, python, java bool

func main() {
	var i int
	c = true
	fmt.Println(i, c, python, java)
}
```

## Variables with initializers

https://go-tour-jp.appspot.com/basics/9

- var 宣言では、変数毎に初期化子( initializer )を与えることができる
- 初期化子が与えられている場合、型を省略できる
- その変数は初期化子が持つ型になる

```go
package main

import "fmt"

var i, j int = 1, 2

func main() {
	var c, python, java = true, false, "yeah!"
	fmt.Println(i, j, c, python, java)
}
```

## Short variable declarations

https://go-tour-jp.appspot.com/basics/10

- 関数の中では、 var 宣言の代わりに、短い := の代入文を使い、暗黙的な型宣言ができる
- 関数の外では、キーワードではじまる宣言( var, func, など)が必要で、 := での暗黙的な宣言は利用できない

```go
package main

import "fmt"

func main() {
	k := 5

	fmt.Println(k)
}
```

## Basic types

https://go-tour-jp.appspot.com/basics/11

Go 言語の基本型(組み込み型)は次のとおり。

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 の別名

rune // int32 の別名
     // Unicode のコードポイントを表す

float32 float64

complex64 complex128
```

## Zero values

https://go-tour-jp.appspot.com/basics/12

- 変数に初期値を与えずに宣言すると、ゼロ値( zero value )が与えられる
- ゼロ値は型によって以下のように与えられる
  - 数値型(int,float など): 0
  - bool 型: false
  - string 型: "" (空文字列( empty string ))

```go
package main

import "fmt"

func main() {
	var i int
	var f float64
	var b bool
	var s string
	fmt.Printf("%v %v %v %q\n", i, f, b, s)
}
```

## Type conversions

https://go-tour-jp.appspot.com/basics/13

- 変数 v 、型 T があった場合、 T(v) は、変数 v を T 型へ変換する
- C 言語とは異なり、Go での型変換は明示的な変換が必要

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y int = 3, 4
	var f float64 = math.Sqrt(float64(x*x + y*y))
	var z uint = uint(f)
	fmt.Println(x, y, z)
}
```

## Type inference

- 明示的な型を指定せずに変数を宣言する場合( := や var = のいずれか)、変数の型は右側の変数から型推論される
- 右側の変数が型を持っている場合、左側の新しい変数は同じ型になる

```go
var i int
j := i // j is an int
```

## Constants

- 定数( constant )は、 const キーワードを使って変数と同じように宣言
- 定数は、文字(character)、文字列(string)、boolean、数値(numeric)のみで使える
- 定数は := を使って宣言できない

```go
package main

import "fmt"

const Pi = 3.1

func main() {
	const World = "世界"
	fmt.Println("Hello", World)
	fmt.Println("Happy", Pi, "Day")

	const Truth = true
	fmt.Println("Go rules?", Truth)
}
```

## Numeric Constants

https://go-tour-jp.appspot.com/basics/16

- 数値の定数は、高精度な 値 ( values )
- 型のない定数は、その状況によって必要な型を取ることになる

## For

- 他の言語、C 言語 や Java、JavaScript の for ループとは異なり、 for ステートメントの 3 つの部分を括る括弧 ( ) はない
- 中括弧 { } は常に必要

```go
package main

import "fmt"

func main() {
	sum := 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)
}
```

## For continued

- 初期化と後処理ステートメントの記述は任意

```go
package main

import "fmt"

func main() {
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```

## For is Go's "while"

https://go-tour-jp.appspot.com/flowcontrol/3

- セミコロン(;)を省略することもできる

```go
package main

import "fmt"

func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```

## Forever

- ループ条件を省略すれば、無限ループ( infinite loop )になりますので、無限ループをコンパクトに表現できる

```go
package main

func main() {
	for {

	}
}
```

## If

- Go 言語の if ステートメントは、先ほどの for ループと同様に、括弧 ( ) は不要で、中括弧 { } は必要

```go
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func main() {
	fmt.Println(sqrt(2), sqrt(-4))
}
```

## If with a short statement

https://go-tour-jp.appspot.com/flowcontrol/6

- if ステートメントは、 for のように、条件の前に、評価するための簡単なステートメントを書くことができる
- ここで宣言された変数は、 if のスコープ内だけで有効

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## If and else

- if ステートメントで宣言された変数は、 else ブロック内でも使うことができる

```go
package main

import (
	"fmt"
	"math"
)

func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## Switch

- switch ステートメントは if - else ステートメントのシーケンスを短く書く方法
- Go では選択された case だけを実行してそれに続く全ての case は実行されない
  - 各 case の最後に必要な break ステートメントが Go では自動的に提供される
- Go の switch の case は定数である必要はなく、 関係する値は整数である必要はない

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

## Switch with no condition

https://go-tour-jp.appspot.com/flowcontrol/11

- 条件のない switch は、 switch true と書くことと同じ
- この switch の構造は、長くなりがちな "if-then-else" のつながりをシンプルに表現できる

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

## ⭐️Defer

https://go-tour-jp.appspot.com/flowcontrol/12

- defer ステートメントは、 defer へ渡した関数の実行を、呼び出し元の関数の終わり(return する)まで遅延させるもの
- defer へ渡した関数の引数は、すぐに評価されますが、その関数自体は呼び出し元の関数が return するまで実行されない

```go
package main

import "fmt"

func main() {
	defer fmt.Println("world")

	fmt.Println("hello")
}
// hello
// world
// worldが最後に表示される
```

## Stacking defers

https://go-tour-jp.appspot.com/flowcontrol/13

- defer へ渡した関数が複数ある場合、その呼び出しはスタック( stack )される
- 呼び出し元の関数が return するとき、 defer へ渡した関数は LIFO(last-in-first-out) の順番で実行される

```go
package main

import "fmt"

func main() {
	fmt.Println("counting")

	for i := 0; i < 10; i++ {
		defer fmt.Println(i)
	}

	fmt.Println("done")
}
```

## Pointers

https://go-tour-jp.appspot.com/moretypes/1

- Go はポインタを扱う
- ポインタは値のメモリアドレスを指す
- 変数 T のポインタは、 \*T 型で、ゼロ値は nil

```go
var p *int
```

& オペレータは、そのオペランド( operand )へのポインタを引き出す。

```go
i := 42
p = &i
```

`*` オペレータは、ポインタの指す先の変数を示す。

```go
fmt.Println(*p) // ポインタpを通してiから値を読みだす
*p = 21         // ポインタpを通してiへ値を代入する
```

これは "dereferencing" または "indirecting" としてよく知られている。

```go
package main

import "fmt"

func main() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j
}
```

## ⭐️Structs

struct (構造体)は、フィールド( field )の集まり

https://go-tour-jp.appspot.com/moretypes/2

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	fmt.Println(Vertex{1, 2})
}
```

## Struct Fields

struct のフィールドは、ドット( . )を用いてアクセスする。

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	v.X = 4
	fmt.Println(v.X)
}
```

## Pointers to structs

https://go-tour-jp.appspot.com/moretypes/4

- struct のフィールドは、struct のポインタを通してアクセスすることもできる
- フィールド X を持つ struct のポインタ p がある場合、フィールド X にアクセスするには (\*p).X のように書くことができる
- しかし、この表記法は大変面倒ですので、Go では代わりに p.X と書くこともできる

```go
package main

import "fmt"

type Vertex struct {
	X int
	Y int
}

func main() {
	v := Vertex{1, 2}
	p := &v
	// どちらの書き方もできる
	(*p).X = 777
	p.Y = 1e9
	fmt.Println(v)
}
// {777 1000000000}
```

## Struct Literals

- struct リテラルは、フィールドの値を列挙することで新しい struct の初期値の割り当てを示している
- Name: 構文を使って、フィールドの一部だけを列挙することができる(この方法でのフィールドの指定順序は関係ない)
  - 例では X: 1 として X だけを初期化している
- & を頭に付けると、新しく割り当てられた struct へのポインタを戻す

```go
package main

import "fmt"

type Vertex struct {
	X, Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	fmt.Println(v1, v2, v3, p)
}
```

## Arrays

https://go-tour-jp.appspot.com/moretypes/6

- [n]T 型は、型 T の n 個の変数の配列( array )を表す
- 以下は、int の 10 個の配列を宣言している
- 配列の長さは、型の一部分ですので、配列のサイズを変えることはできない
- これは制約のように思えますが、心配しないでください。Go は配列を扱うための便利な方法を提供しています

```go
package main

import "fmt"

func main() {
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```

## Slices

- 配列は固定長
- 一方で、スライスは可変長
- より柔軟な配列と見なすこともできる
- 実際には、スライスは配列よりもより一般的
- 型 []T は 型 T のスライスを表す
- コロンで区切られた二つのインデックス low と high の境界を指定することによってスライスが形成される
- これは最初の要素は含むが、最後の要素は除いた半開区間を選択する

```go
a[low : high]
```

- 次の式は a の要素の内 1 から 3 を含むスライスを作る

```go
a[1:4]
```

## Slices are like references to arrays

https://go-tour-jp.appspot.com/moretypes/8

- スライスは配列への参照のようなもの
- スライスはどんなデータも格納しておらず、単に元の配列の部分列を指し示している
- スライスの要素を変更すると、その元となる配列の対応する要素が変更される
- 同じ元となる配列を共有している他のスライスは、それらの変更が反映される

```go
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```

## Slice literals

- スライスのリテラルは長さのない配列リテラルのようなもの
- 配列リテラル

```go
[3]bool{true, true, false}
```

- 上記と同様の配列を作成し、それを参照するスライス

```go
[]bool{true, true, false}
```

## Slice defaults

https://go-tour-jp.appspot.com/moretypes/10

- スライスするときは、それらの既定値を代わりに使用することで上限または下限を省略することができる
- 既定値は下限が 0 で上限はスライスの長さ
- 以下の配列において

```go
var a [10]int
```

これらのスライス式は等価。

```go
a[0:10]
a[:10]
a[0:]
a[:]
```

## Slice length and capacity

https://go-tour-jp.appspot.com/moretypes/11

- スライスは長さ( length )と容量( capacity )の両方を持っている
- スライスの長さは、それに含まれる要素の数
- スライスの容量は、スライスの最初の要素から数えて、元となる配列の要素数
- スライス s の長さと容量は len(s) と cap(s) という式を使用して得ることができる

```go
package main

import "fmt"

func main() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s)

	// Slice the slice to give it zero length.
	s = s[:0]
	printSlice(s)

	// Extend its length.
	s = s[:4]
	printSlice(s)

	// Drop its first two values.
	s = s[2:]
	printSlice(s)
}

func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}
```

## Nil slices

https://go-tour-jp.appspot.com/moretypes/12

- スライスのゼロ値は nil
- nil スライスは 0 の長さと容量を持っており、何の元となる配列も持っていない

```go
package main

import "fmt"

func main() {
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}
```

## Creating a slice with make

https://go-tour-jp.appspot.com/moretypes/13

## Range

https://go-tour-jp.appspot.com/moretypes/16

- for ループに利用する `range` は、スライスや、マップ( `map` )をひとつずつ反復処理するために使う
- スライスを range で繰り返す場合、range は反復毎に 2 つの変数を返す
- 1 つ目の変数はインデックス( index )で、2 つ目はインデックスの場所の要素のコピー

```go
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

## Go: 参考資料

- [A Tour of Go](https://go-tour-jp.appspot.com/welcome/1)
- [もしもいま、Go をイチから学ぶならどうしたい？　松木雅幸 / Songmu さんが考える学習ロードマップ](https://findy-code.io/engineer-lab/techtensei-songmu)
- [とってもやさしい Go 言語入門](https://zenn.dev/ak/articles/1fb628d82ed79b)
- [プログラミング言語 Go 完全入門](https://docs.google.com/presentation/d/1RVx8oeIMAWxbB7ZP2IcgZXnbZokjCmTUca-AbIpORGk/edit?pli=1&slide=id.g4f417182ce_0_0#slide=id.g4f417182ce_0_0)
