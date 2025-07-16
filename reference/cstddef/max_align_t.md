# max_align_t
* cstddef[meta header]
* std[meta namespace]
* type-alias[meta id-type]
* cpp11[meta cpp]

```cpp
namespace std {
  using max_align_t = object-type;
}
```
* object-type[italic]

## 概要
`std::max_align_t`のアライメントサイズは、スカラー型の中の最も大きなアライメントサイズと同じかそれ以上である。

この型は、C++11では[POD型](/reference/type_traits/is_pod.md)、C++20では[トリビアル型](/reference/type_traits/is_trivial.md)に分類される。

`alignas`指示子により、`std::max_align_t`のアライメントより大きいサイズのアライメントを指定できるかどうかは、実装依存である。


## 例
```cpp example
#include <iostream>
#include <cstddef>
#include <type_traits>

int main()
{
  std::cout << "sizeof(max_align_t): " << sizeof(std::max_align_t) << std::endl;
  std::cout << "alignof(max_align_t): " << alignof(std::max_align_t) << std::endl;
  static_assert(alignof(std::max_align_t) >= alignof(long long), "");
  static_assert(alignof(std::max_align_t) >= alignof(long double), "");

  std::cout << "is_object<max_align_t>: " << std::is_object<std::max_align_t>::value << std::endl;
  std::cout << "is_scalar<max_align_t>: " << std::is_scalar<std::max_align_t>::value << std::endl;
  std::cout << "is_union<max_align_t>: " << std::is_union<std::max_align_t>::value << std::endl;
  std::cout << "is_array<max_align_t>: " << std::is_array<std::max_align_t>::value << std::endl;
  std::cout << "is_class<max_align_t>: " << std::is_class<std::max_align_t>::value << std::endl;
  std::cout << "is_pod<max_align_t>: " << std::is_pod<std::max_align_t>::value << std::endl;
}
```
* std::max_align_t[color ff0000]
* std::is_object[link /reference/type_traits/is_object.md]
* std::is_scalar[link /reference/type_traits/is_scalar.md]
* std::is_union[link /reference/type_traits/is_union.md]
* std::is_array[link /reference/type_traits/is_array.md]
* std::is_class[link /reference/type_traits/is_class.md]
* std::is_pod[link /reference/type_traits/is_pod.md]

### 出力例
```
sizeof(max_align_t): 24
alignof(max_align_t): 8
is_object<max_align_t>: 1
is_scalar<max_align_t>: 0
is_union<max_align_t>: 0
is_array<max_align_t>: 0
is_class<max_align_t>: 1
is_pod<max_align_t>: 1
```


## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang): 3.5 [mark verified]
- [GCC](/implementation.md#gcc): 4.9 [mark verified]
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2012 [mark verified], 2013 [mark verified], 2015 [mark verified]


## 関連項目
- [C++20 PODを非推奨化](/lang/cpp20/deprecate_pod.md)


## 参照
- [P0767R1 Deprecate POD](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/p0767r1.html)
    - C++20でPOD用語の非推奨化にともない、この型がPOD型ではなくトリビアル型に分類されるよう規定が変更された
