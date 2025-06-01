# set_value
* execution[meta header]
* cpo[meta id-type]
* std::execution[meta namespace]
* cpp26[meta cpp]

```cpp
namespace std::execution {
  struct set_value_t { unspecified };  // タグ型

  inline constexpr set_value_t set_value{};
}
```
* unspecified[italic]

## 概要
`set_value`は、非同期操作の正常完了を表現する値(value)完了関数である。完了関数の呼び出しは完了操作と呼ばれる。

値完了関数には完了タグ`set_value_t`が関連付けられ、完了操作の値完了シグネチャは戻り値型`set_value_t`と任意個の引数を持つ関数型として表現される。


## 効果
- 引数`rcvr`が左辺値またはconst右辺値の場合、式`set_value(rcvr, vs...)`は不適格となる。
- そうでなければ、`rcvr.set_value(vs...)`と等価である。


## 例外
投げない


## カスタマイゼーションポイント
[Receiver型](receiver.md)の非const右辺値`rcvr`に対して式`rcvr.set_value(vs...)`が呼び出される。
このとき、`noexcept(rcvr.set_value(vs...)) == true`であること。


## 説明専用エンティティ
### `SET-VALUE`
説明用の式`rcvr`, `expr`に対して、説明専用の式`SET-VALUE(rcvr, expr)`は下記と等価である。

- `expr`の型が`void`のとき、式`(expr, set_value(`[`std::move`](/reference/utility/move.md)`(rcvr)))`
- そうでなければ、式`set_value(`[`std::move`](/reference/utility/move.md)`(rcvr), expr)`

### `TRY-EVAL`
説明用の式`rcvr`, `expr`に対して、説明専用の式`TRY-EVAL(rcvr, expr)`は下記と等価である。

- `expr`が潜在的に例外送出するならば、下記と等価。

    ```cpp
    try {
      expr;
    } catch(...) {
      set_error(std::move(rcvr), current_exception());
    }
    ```
    * set_error[link set_error.md]
    * std::move[link /reference/utility/move.md]
    * current_exception()[link /reference/exception/current_exception.md]

- そうでなければ、式`expr`

### `TRY-SET-VALUE`
説明用の式`rcvr`, `expr`に対して、説明専用の式`TRY-SET-VALUE(rcvr, expr)`は`rcvr`が1回だけ評価されることを除いて、下記と等価。

```cpp
TRY-EVAL(rcvr, SET-VALUE(rcvr, expr))
```

### `SET-VALUE-SIG`
説明用の型`T`に対して、説明専用の型`SET-VALUE-SIG(T)`を下記の通り定義する。

- `T`がCV修飾された`void`ならば、型`set_value_t()`
- そうでなければ、型`set_value_t(T)`


## 備考
完了関数`set_value`は[Sender](sender.md)内部実装から呼び出される想定であり、実行制御ライブラリ利用者が直接利用する必要はない。


## 例
```cpp example
#include <execution>
namespace ex = std::execution;

struct ValueReceiver {
  using receiver_concept = ex::receiver_t;

  // 値完了シグネチャ set_value_t(int, int)
  void set_value(int, int) && noexcept {}
};

int main()
{
  ValueReceiver rcvr;
  ex::set_value(std::move(rcvr), 1, 2);
}
```
* ex::set_value[color ff0000]
* ex::receiver_t[link receiver.md]
* std::move[link /reference/utility/move.md]

### 出力
```
```


## バージョン
### 言語
- C++26

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 関連項目
- [`execution::receiver`](receiver.md)
- [`execution::set_error_t`](set_error.md)
- [`execution::set_stopped_t`](set_stopped.md)


## 参照
- [P2300R10 `std::execution`](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2024/p2300r10.html)
