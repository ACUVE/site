# clear
* map[meta header]
* std[meta namespace]
* map[meta class]
* function[meta id-type]

```cpp
void clear();           // C++03
void clear() noexcept;  // C++11
```

## 概要
`map` コンテナ内の全ての要素を削除する。それぞれのデストラクタが呼ばれ、コンテナから削除される。[`size()`](size.md) は 0 になる。


## 計算量
線形時間

## 例外

投げない

## 例
```cpp example
#include <iostream>
#include <map>

int main() 
{
  std::map<int, char> m;
  m[3] = 'C';
  m[4] = 'D';
  m[1] = 'A';
  m[2] = 'B';

  std::cout << m.size() << std::endl;

  m.clear();

  std::cout << m.size() << std::endl;

  return 0;
}
```
* clear()[color ff0000]
* m.size()[link size.md]

### 出力
```
4
0
```

## バージョン
### 言語
- C++03

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2012 [mark verified]

## 関連項目

| 名前 | 説明 |
|-------------------------------------------------------------------------------------|-----------------------------------------------------|
| [`map::erase`](erase.md) | 要素を削除する |
| [`map::size`](size.md) | 要素数を取得する |
| [`map::empty`](empty.md) | コンテナが空であるかどうかを調べる |
