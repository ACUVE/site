# islessequal
* cmath[meta header]
* std[meta namespace]
* function[meta id-type]
* cpp11[meta cpp]

```cpp
namespace std {
  bool islessequal(float x, float y);             // (1) C++11からC++20まで
  bool islessequal(double x, double y);           // (2) C++11からC++20まで
  bool islessequal(long double x, long double y); // (3) C++11からC++20まで

  constexpr bool
    islessequal(floating-point-type x,
                floating-point-type y);           // (4) C++23

  bool
    islessequal(Integral x, Integral y);          // (5) C++11
  constexpr bool
    islessequal(Integral x, Integral y);          // (5) C++23
}
```
* Integral[italic]

## 概要
浮動小数点数について、左辺が右辺以下かを判定する。

- (1) : `float`に対するオーバーロード
- (2) : `double`に対するオーバーロード
- (3) : `long double`に対するオーバーロード
- (4) : 浮動小数点数型に対するオーバーロード
- (5) : 整数型に対するオーバーロード (`double`にキャストして計算される)


## 戻り値
`x <= y`と等価の演算によって、`x`が`y`以下であれば`true`、そうでなければ`false`を返す。

`x <= y`と違って、この関数は`x`と`y`が順序付けされていない場合に、[`FE_INVALID`](/reference/cfenv/fe_invalid.md)（無効演算浮動小数点例外）は発生しない。


## 備考
- C標準ライブラリでは本関数は関数マクロとして定義されるが、C++標準ライブラリでは関数として定義される
- C標準ライブラリでは本関数は`int`型を戻り値とするが、C++標準ライブラリでは`bool`型を戻り値とする
- C++23では、(1), (2), (3)が(4)に統合され、拡張浮動小数点数型を含む浮動小数点数型へのオーバーロードとして定義された


## 例
```cpp example
#include <iostream>
#include <cmath>
#include <limits>

void test(double x, double y)
{
  std::cout << std::boolalpha;
  std::cout << "islessequal(" << x << ", " << y << ") = " << std::islessequal(x, y) << std::endl;
}

int main()
{
  test(1.0, 2.0);
  test(2.0, 1.0);
  test(2.0, 2.0);

  test(-0.0, 0.0);

  const double inf = std::numeric_limits<double>::infinity();
  const double nan = std::numeric_limits<double>::quiet_NaN();

  test(1.0, inf);
  test(inf, 1.0);
  test(inf, inf);

  test(1.0, nan);
  test(nan, 1.0);
  test(nan, nan);
}
```
* std::islessequal[color ff0000]
* infinity()[link /reference/limits/numeric_limits/infinity.md]
* quiet_NaN()[link /reference/limits/numeric_limits/quiet_nan.md]

### 出力例
```
islessequal(1, 2) = true
islessequal(2, 1) = false
islessequal(2, 2) = true
islessequal(-0, 0) = true
islessequal(1, inf) = true
islessequal(inf, 1) = false
islessequal(inf, inf) = true
islessequal(1, nan) = false
islessequal(nan, 1) = false
islessequal(nan, nan) = false
```

## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang): 3.0 [mark verified]
- [GCC](/implementation.md#gcc): 4.3 [mark verified]
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 参照
- [P0533R9 constexpr for `<cmath>` and `<cstdlib>`](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2021/p0533r9.pdf)
    - C++23での、一部関数の`constexpr`対応
- [P1467R9 Extended floating-point types and standard names](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p1467r9.html)
    - C++23で導入された拡張浮動小数点数型への対応として、`float`、`double`、`long double`のオーバーロードを`floating-point-type`のオーバーロードに統合し、拡張浮動小数点数型も扱えるようにした
