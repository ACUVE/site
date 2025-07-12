# 定数式内での非リテラル変数、静的変数・スレッドローカル変数およびgotoとラベルの存在を許可する [P2242R3]
* cpp23[meta cpp]

<!-- start lang caution -->

このページはC++23に採用された言語機能の変更を解説しています。

のちのC++規格でさらに変更される場合があるため[関連項目](#relative-page)を参照してください。

<!-- last lang caution -->

## 概要

`constexpr`関数が非リテラル変数、静的変数・スレッドローカル変数およびgoto文とラベルを含むことを許可する。

ただし、これをコンパイル時に評価した場合、従来通りエラーとなる。
これらを含むコードパスは実行時にのみ通過するようにしなければならない。

## この機能が必要になった背景・経緯

これまでは、`constexpr`関数内には非リテラル変数、静的変数・スレッドローカル変数およびgoto文とラベルが含まれてはならなかった。
これは、`constexpr`関数が定数式実行できない要素を含んではならないという意味で、理にかなった制限であった。

しかし、C++20で[`std::is_constant_evaluated`](/reference/type_traits/is_constant_evaluated.md)によって定数式実行されるコードパスと実行時に通るコードパスを分けることが可能になり、状況が変化した。
定数式実行できないコードとはいえ、実行時にのみ通りコンパイル時には通らないコードパスに含まれていた場合にもプログラムが不適格となる、というのは直観に反する。

よって、コンパイル時に評価されない限りにおいて、`constexpr`関数内に非リテラル変数、静的変数・スレッドローカル変数およびgoto文とラベルを含むことが許可されることとなった。

ただし、`constexpr`内に存在することが許可されたものの定数式として評価できるわけではないので、これらの機能は依然としてコンパイル時に使用することはできない。

## <a id="relative-page" href="#relative-page">関連項目</a>

- [C++20 - constexpr関数内でのtry-catchブロックを許可](/lang/cpp20/try-catch_blocks_in_constexpr_functions.md)
- [C++20 - constexpr関数内で未評価のインラインアセンブリを許可することによる組み込み関数のconstexpr有効化](/lang/cpp20/enabling_constexpr_intrinsics_by_permitting_unevaluated_inline-assembly_in_constexpr_functions.md)
- [C++23 - `constexpr`関数内での`static constexpr`変数を許可](/lang/cpp23/permitting_static_constexpr_variables_in_constexpr_functions.md)

## 参照

- [P2242R3 - Non-literal variables (and labels and gotos) in constexpr functions](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2021/p2242r3.html)
