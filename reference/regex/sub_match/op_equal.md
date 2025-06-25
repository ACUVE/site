# operator==
* regex[meta header]
* std[meta namespace]
* function template[meta id-type]
* cpp11[meta cpp]

```cpp
namespace std {
  template <class BiIter>
    bool operator==(
      const sub_match<BiIter>& lhs,
      const sub_match<BiIter>& rhs);                            // (1) C++11

  template <class BiIter, class ST, class SA>
    bool operator==(
      const sub_match<BiIter>& lhs,
      const basic_string<
        typename iterator_traits<BiIter>::value_type,
        ST,
        SA
      >& rhs);                                                  // (2) C++11

  template <class BiIter>
    bool operator==(
      const sub_match<BiIter>& lhs,
      const typename iterator_traits<BiIter>::value_type& rhs); // (3) C++11

  template <class BiIter>
    bool operator==(
      const sub_match<BiIter>& lhs,
      const typename iterator_traits<BiIter>::value_type* rhs); // (4) C++11

  // operator==により、以下のオーバーロードが使用可能になる (C++20)
  template <class BiIter, class ST, class SA>
    bool operator==(
      const basic_string<
        typename iterator_traits<BiIter>::value_type,
        ST,
        SA
      >& lhs,
      const sub_match<BiIter>& rhs);                            // (5) C++11

  template <class BiIter>
    bool operator==(
      const typename iterator_traits<BiIter>::value_type* lhs,
      const sub_match<BiIter>& rhs);                            // (6) C++11

  template <class BiIter>
    bool operator==(
      const typename iterator_traits<BiIter>::value_type& lhs,
      const sub_match<BiIter>& rhs);                            // (7) C++11
}
```

## 概要
マッチした文字列を等値比較する。


## 戻り値
- (1) `lhs.`[`compare`](compare.md)`(rhs) == 0`
- (2) `lhs.`[`compare`](compare.md)`(typename` [`sub_match`](../sub_match.md)`<BiIter>::string_type(rhs.`[`data`](../../string/basic_string/data.md)`(), rhs.`[`size`](../../string/basic_string/size.md)`())) == 0`  
- (3) `lhs.`[`compare`](compare.md)`(rhs) == 0`
- (4) `lhs.`[`compare`](compare.md)`(typename` [`sub_match`](../sub_match.md)`<BiIter>::string_type(1, rhs)) == 0`
- (5) `rhs.`[`compare`](compare.md)`(typename` [`sub_match`](../sub_match.md)`<BiIter>::string_type(lhs.`[`data`](../../string/basic_string/data.md)`(), lhs.`[`size`](../../string/basic_string/size.md)`())) == 0`  
    なお、C++11 の規格書では `rhs.`[`compare`](compare.md)`(lhs.`[`c_str`](../../string/basic_string/c_str.md)`()) == 0` となっているが、この式では `lhs` に `'\0'` が含まれていた場合に正しく比較することができないため、規格上の誤りとして上記の式に修正された。
    なお、C++11 の規格書では `lhs.`[`compare`](compare.md)`(rhs.`[`c_str`](../../string/basic_string/c_str.md)`()) == 0` となっているが、この式では `rhs` に `'\0'` が含まれていた場合に正しく比較することができないため、規格上の誤りとして上記の式に修正された。
- (6) `rhs.`[`compare`](compare.md)`(lhs) == 0`
- (7) `rhs.`[`compare`](compare.md)`(typename` [`sub_match`](../sub_match.md)`<BiIter>::string_type(1, lhs)) == 0`

## 備考
- (1) の形式でもマッチした文字列のみが比較され、マッチした位置は考慮されない。（例を参照）
- (2)、および、(5) の形式でも比較に使用する文字特性クラスは標準の [`char_traits`](../../string/char_traits.md)`<value_type>` が使用され、テンプレート引数に指定された文字特性クラス `ST` は考慮されない。
- この演算子により、以下の演算子が使用可能になる (C++20)：
    - `operator!=`


## 例
```cpp example
#include <iostream>
#include <regex>
#include <string>

int main()
{
  const char ca[] = "abc abc";
  const std::regex re(R"((\w+) (\w+))");

  std::cmatch m;
  if (std::regex_search(ca, m, re)) {
    std::csub_match sub1 = m[1];
    std::csub_match sub2 = m[2];
    const std::string s1 = "abc";
    const std::string s2 = "ABC";
    std::cout << std::boolalpha
              << (sub1  == sub2 ) << std::endl  // (1) の形式
              << (sub1  == s2   ) << std::endl  // (2) の形式
              << (sub1  == "abc") << std::endl  // (3) の形式
              << (sub1  == 'a'  ) << std::endl  // (4) の形式
              << (s1    == sub2 ) << std::endl  // (5) の形式
              << ("ABC" == sub2 ) << std::endl  // (6) の形式
              << ('A'   == sub2 ) << std::endl; // (7) の形式
  } else {
    std::cout << "not match" << std::endl;
  }
}
```
* ==[color ff0000]
* std::regex[link ../basic_regex.md]
* std::cmatch[link ../match_results.md]
* std::regex_search[link ../regex_search.md]
* std::csub_match[link ../sub_match.md]

### 出力
```
true
false
true
false
true
false
false
```


## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang): 3.0 [mark verified], 3.1 [mark verified], 3.2 [mark verified], 3.3 [mark verified], 3.4 [mark verified], 3.5 [mark verified], 3.6 [mark verified]
- [GCC](/implementation.md#gcc): 4.9.0 [mark verified], 4.9.1 [mark verified], 5.0.0 [mark verified]
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??

## 参照
- [P1614R2 The Mothership has Landed](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1614r2.html)
    - C++20での三方比較演算子の追加と、関連する演算子の自動導出
