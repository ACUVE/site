# min
* random[meta header]
* std[meta namespace]
* random_device[meta class]
* function[meta id-type]
* cpp11[meta cpp]

```cpp
static constexpr result_type min();
```

## 概要
生成する値の最小値を取得する。


## 戻り値
[`numeric_limits`](/reference/limits/numeric_limits.md)`<result_type>::`[`min()`](/reference/limits/numeric_limits/min.md)


## 計算量
定数時間


## 例
```cpp example
#include <iostream>
#include <random>

int main()
{
  unsigned int min_value = std::random_device::min();
  std::cout << min_value << std::endl;
}
```
* min()[color ff0000]

### 出力
```
0
```

## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): 4.7.2 [mark verified]
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2010 [mark verified], 2012 [mark verified], 2013 [mark verified], 2015 [mark verified], 2017 [mark verified]


## 参照
