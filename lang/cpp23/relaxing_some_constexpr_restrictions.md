# constexpr関数が定数実行できない場合でも適格とする [P2448R2]
* cpp23[meta cpp]

<!-- start lang caution -->

このページはC++23に採用された言語機能の変更を解説しています。

のちのC++規格でさらに変更される場合があるため[関連項目](#relative-page)を参照してください。

<!-- last lang caution -->

## 概要

いかなる呼び出しにおいても定数式実行できない`constexpr`関数が存在しても、プログラムが不適格にならないようにする。

ただし、このような関数の存在までは許容するというだけで、定数式実行できない`constexpr`関数を定数式文脈で呼び出すと従来通りエラーとなる。

## この機能が必要になった背景・経緯

これまでは、いかなる実引数での呼び出しでも定数式実行できない`constexpr`関数が存在するだけでプログラムは不適格になっていた。

例えば以下のような場合である。

```cpp
int f(int x) { return x + 1; }

constexpr int g(int x) { return f(x); } // error! fはいかなるxについても定数式実行不可能
```

`g`は`constexpr`でない関数`f`を呼び出すため、プログラムの中で実際に`g`が定数式実行されない場合であっても、このコードは不適格である。
これによって、定数式実行できない言語機能を使用している関数や、`constexpr`でない関数を誤って呼び出すようなミスを検出することができた。

しかし、多くの標準ライブラリが`constexpr`対応を進めていくようになり、状況が変化した。

例えば、`std::optional`の`reset`メンバメソッドが`constexpr`に対応するのはC++23以降である。
これは`std::optional`の内部実装が`union`のアクティブメンバを更新しているからであり、これを定数式内で実行するにはC++20を待たねばならなかった（参考：[定数式内での共用体のアクティブメンバの変更を許可](/lang/cpp20/changing_the_active_member_of_a_union_inside_constexpr.md)）。

よって、以下のコードはC++17では不適格だが、C++23では正しいコードとなる。

```cpp
template<typename T>
constexpr void f(std::optional<T>& opt)
{
  opt.reset();
}
```

多くのバージョンで使われることが想定されるライブラリにおいてこれを最大限活用できるよう記述するには、`constexpr`指定するかどうかをマクロで変更しなければならない。
例えばC++23から`constexpr`になる関数群に対して以下のようなマクロを使うか、

```cpp
#if __cplusplus >= 202300L // 具体的な値は未定
#  define MYLIB_CXX23_CONSTEXPR constexpr
#else
#  define MYLIB_CXX23_CONSTEXPR
#endif

template<typename T>
MYLIB_CXX23_CONSTEXPR void f(std::optional<T>& opt)
{
  opt.reset();
}
```

あるいはより細かく指定するためにそれぞれの機能テストマクロを使うしかなかった。

```cpp

template<typename T>
#if __cpp_lib_optional >= 202106
constexpr
#endif
void f(std::optional<T>& opt)
{
  opt.reset();
}
```

これはいかにも冗長であり、またライブラリの導入時期だけでなくその関数一つ一つが`constexpr`指定されたタイミングを意識する必要があるため、正しく指定するのは非常に難しい。

`constexpr`指定を行わないことによってもこの問題は回避できるが、自身以外のユーザーが存在するライブラリの場合は`constexpr`対応が望まれる可能性があり、その場合はこの問題に対処する必要が生じる。

多くの場合、標準ライブラリ関数が`constexpr`指定されるのは、その機能が定数式実行可能になったバージョンよりも遅れる。
例えば、`std::array`の`operator[]`の非`const`版が`constexpr`指定されるのは、定数式内で変数の変更が許可されるC++14ではなく、その次のC++17である（[参考](/reference/array/array/op_at.md)）。
このような状況では上記のような解決策を用いてもミスを避けることは容易ではない。

現在、登場時点では定数式内で実行できなかったために`constexpr`されていなかった多くの標準ライブラリ関数が、のちにコア言語機能が追加されて`constexpr`指定されている。
そのため、現時点で`constexpr`ではない関数も次のバージョンで`constexpr`になるかもしれず、よって関数が`constexpr`指定されているだけでエラーにする意義は薄れてきた。

以上から、コンパイル時に実行できない`constexpr`をエラーとするのは、実際にコンパイル時に呼びだされてからでよい、ということになった。

実際に定数式内で実行できない関数が定数式内で呼び出された場合、これは従来通りエラーとするほかない。
しかし、定数式内で呼び出されていないのならば、定数式内で実行できない関数が存在していてもプログラムを不適格とはしない。

## 仕様

`constexpr`指定された関数が満たすべき条件を緩和する。

- 関数の戻り値は`literal`型でなくともよい。
- 関数の実引数はどれも`literal`型でなくともよい。
- いかなる実引数が与えられても定数式実行不可能でもよい
- いかなる`template`実引数が与えられても定数式実行不可能でもよい

`constexpr`指定された、`=delete`指定されていないコンストラクタが満たすべき条件を緩和する。

- 非静的メンバ変数のコンストラクタは`constexpr`でなくともよい
- [委譲コンストラクタ](/lang/cpp11/delegating_constructors.md)の場合、委譲先のコンストラクタが`constexpr`でなくともよい

`constexpr`指定された、`=delete`指定されていないデストラクタが満たすべき条件を緩和する。

- 非静的メンバ変数のデストラクタは`constexpr`でなくともよい

また、陽に`default`指定された関数は、それが`constexpr-suitable`である限り、暗黙に`constexpr`指定される。
`constexpr-suitable`とは、コルーチン関数ではなく、仮想基底クラスを持つクラスのコンストラクタまたはデストラクタでもない関数を指す。

## <a id="relative-page" href="#relative-page">関連項目</a>

- [C++11 `constexpr`](/lang/cpp11/constexpr.md)
- [C++20 `constexpr`関数内での`try-catch`ブロックを許可](/lang/cpp20/try-catch_blocks_in_constexpr_functions.md)
- [C++20 `constexpr`関数内で未評価のインラインアセンブリを許可することによる組み込み関数の`constexpr`有効化](/lang/cpp20/enabling_constexpr_intrinsics_by_permitting_unevaluated_inline-assembly_in_constexpr_functions.md)

## 参照

- [P2448R2 - Relaxing some constexpr restrictions](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2448r2.html)
