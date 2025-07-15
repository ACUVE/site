# submdspan_mapping
* mdspan[meta header]
* function template[meta id-type]
* std[meta namespace]
* layout_stride::mapping[meta class]
* cpp26[meta cpp]

```cpp
template<class... SliceSpecifiers>
constexpr auto submdspan-mapping-impl(  // exposition only
  SliceSpecifiers ... slices) const -> see below;

template<class... SliceSpecifiers>
friend constexpr auto submdspan_mapping(
  const mapping& src, SliceSpecifiers... slices)
{
  return src.submdspan-mapping-impl(slices...);
}
```
* submdspan-mapping-impl[italic]

## 概要
[`submdspan`](../../submdspan.md)関数をサポートするためのカスタマイゼーションポイント。

説明用の型`index_type`を[`Extents::index_type`](../../extents.md)、型`S_k`を`SliceSpecifiers`の`k`番目の型とする。


## テンプレートパラメータ制約
`sizeof...(slices)`が[`Extents::rank()`](../../extents/rank.md)と等しいこと。


## 適格要件
`extents()`の各次元インデクス`k`において、下記いずれかのうち1つだけを満たすこと。

- 型`S_k`が[`convertible_to`](/reference/concepts/convertible_to.md)`<index_type>`のモデル
- 型`S_k`が[`index-pair-like`](../../index-pair-like.md)`<index_type>`のモデル
- [`is_convertible_v`](/reference/type_traits/is_convertible.md)`<S_k,` [`full_extent_t`](../../full_extent_t.md)`>`が`true`
- 型`S_k`が[`strided_slice`](../../strided_slice.md)の特殊化


## 事前条件
`extents()`の各次元インデクス`k`において、`s_k`を`slices`の`k`番目の値としたとき、下記を全て満たすこと。

- 型`S_k`が[`strided_slice`](../../strided_slice.md)の特殊化のとき
    - `s_k.extent == 0`、または
    - `s_k.stride > 0`
- `0` ≤ [`first_`](../../first_.md)`<index_type, k>(slices...)` ≤ [`last_`](../../last_.md)`<k>(extents(), slices...)` ≤ `extents().`[`extent(k)`](../../extents/extent.md)


## 戻り値
説明用の値や型を次の通り定義する。

- 値`sub_ext` : 式[`submdspan_extents`](../../submdspan_extents.md)`(extents(), slices...)`の結果
- 型`SubExtents` : `decltype(sub_ext)`
- 値`sub_strides` : `extents()`の各次元インデクス`k`において、[`map-rank[k]`](../../submdspan_extents.md)が[`dynamic_extent`](/reference/span/dynamic_extent.md)ではない`k`に対し`sub_strides[map-rank[k]]`が下記を満たす、[`array`](/reference/array/array.md)`<SubExtents::index_type,` [`SubExtents::rank()`](../../extents/rank.md)`>`型の配列値
    - 型`S_k`が[`strided_slice`](../../strided_slice.md)の特殊化かつ`s_k.stride < s_k.extent`の場合、[`stride(k)`](stride.md) `*` [`de-ice`](../../de-ice.md)`(s_k.stride)`
    - そうでなければ、[`stride(k)`](stride.md)
- パラメータパック`P` : [`is_same_v`](/reference/type_traits/is_same.md)`<`[`make_index_sequence`](/reference/utility/make_index_sequence.md)`<rank()>,` [`index_sequence`](/reference/utility/index_sequence.md)`<P...>> == true`
- 値`offset` : `size_t`型の値[`(*this)`](op_call.md)`(`[`first_`](../../first_.md)`<index_type, P>(slices...)...)`

説明専用の`submdspan-mapping-impl`関数テンプレートは下記の値を返す。

- [`Extents::rank()`](../../extents/rank.md) `== 0`のとき、[`submdspan_mapping_result`](../../submdspan_mapping_result.md)`{*this, 0}`
- [`submdspan_mapping_result`](../../submdspan_mapping_result.md)`{`[`layout_stride::mapping`](../../layout_stride.md)`(sub_ext, sub_strides), offset}`


## バージョン
### 言語
- C++26

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 関連項目
- [`submdspan`](../../submdspan.md)


## 参照
- [P2630R4 Submdspan](https://open-std.org/jtc1/sc22/wg21/docs/papers/2023/p2630r4.html)
