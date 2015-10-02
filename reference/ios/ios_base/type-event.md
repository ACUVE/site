#event
* ios[meta header]
* typedef[meta id-type]
* std[meta namespace]
* ios_base[meta class]

```cpp
enum event { erase_event, imbue_event, copyfmt_event };
```

##概要
`event` は　[`register_callback`](register_callback.md.nolink) で登録したコールバック関数を呼び出す際に、当該関数を呼び出すきっかけとなったイベントの種類を表すための列挙型である。  
`event` には以下の表のような列挙子値が存在する。

| 列挙子          | 発生したイベント |
|-----------------|----------------------|
| `erase_event`   | [`ios_base`](../ios_base.md)`::`[`~ios_base`](op_destructor.md.nolink) が呼び出されてオブジェクトの破棄を開始した。または、[`basic_ios`](../basic_ios.md)`::`[`copyfmt`](copyfmt.md.nolink) が呼び出された。 |
| `imbue_event`   | [`ios_base`](../ios_base.md)`::`[`imbue`](imbue.md.nolink) が呼び出されて [`ios_base`](../ios_base.md)`::`[`getloc`](getloc.md.nolink) が新しく設定されたロケールオブジェクトを返すようになった。 |
| `copyfmt_event` | [`basic_ios`](../basic_ios.md)`::`[`copyfmt`](../basic_ios/copyfmt.md.nolink) が呼び出されて [`basic_ios`](../basic_ios.md)`::`[`exceptions`](../basic_ios/exceptions.md.nolink) 以外の設定がコピーされた。 |

なお、各イベントが発生する正確なタイミングは、それぞれの関数を参照。


##例
```cpp
#include <iostream>
#include <sstream>

std::stringstream ss1;

void f(std::ios_base::event e, std::ios_base& str, int index)
{
  switch (e) {
  case std::ios_base::erase_event:
    std::cout << "event = erase_event, ";
    break;
  case std::ios_base::copyfmt_event:
    std::cout << "event = copyfmt_event, ";
    break;
  case std::ios_base::imbue_event:
    std::cout << "event = imbue_event, ";
    break;
  }
  std::cout << "str = " << (&str == &ss1 ? "ss1" : "ss2") << ", index = " << index << ", getloc.name = " << str.getloc().name() << '\n';
}

int main()
{
  ss1.register_callback(f, 0);
  std::locale l("en_US.UTF-8");

  {
    std::stringstream ss2;
    ss2.register_callback(f, 1);

    std::cout << "imbue start\n";
    ss2.imbue(l);
    std::cout << "imbue end\n";

    std::cout << "copyfmt start\n";
    ss2.copyfmt(ss1);
    std::cout << "copyfmt end\n";
  }
  std::cout << "block end\n";
}
```
* iostream[link ../../iostream.md]
* sstream[link ../../sstream.md]
* ios_base[link ../ios_base.md]
* basic_ios[link ../basic_ios.md]
* stringstream[link ../../sstream/basic_stringstream.md.nolink]
* cout[link ../../iostream/cout.md]
* event[color ff0000]
* erase_event[color ff0000]
* copyfmt_event[color ff0000]
* imbue_event[color ff0000]
* locale[link ../../locale.md]
* register_callback[link register_callback.md.nolink]
* imbue[link imbue.md.nolink]
* copyfmt[link ../basic_ios/copyfmt.md.nolink]

###出力
```
imbue start
event = imbue_event, str = ss2, index = 1, getloc.name = en_US.UTF-8
imbue end
copyfmt start
event = erase_event, str = ss2, index = 1, getloc.name = en_US.UTF-8
event = copyfmt_event, str = ss2, index = 0, getloc.name = C
copyfmt end
event = erase_event, str = ss2, index = 0, getloc.name = C
block end
event = erase_event, str = ss1, index = 0, getloc.name = C
```


##バージョン
##言語
- C++98

###処理系
- [Clang](/implementation.md#clang): 3.0, 3.1, 3.2, 3.3, 3.4, 3.5.0, 3.6.0, 3.7.0, 3.8.0
- [GCC](/implementation.md#gcc): 4.3.6, 4.4.7, 4.5.4, 4.6.4, 4.7.3, 4.8.1, 4.8.2, 4.9.0, 4.9.1, 4.9.2, 5.1.0, 5.2.0, 6.0.0
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


##参照
- [`ios_base`](../ios_base.md)`::`[`register_callback`](register_callback.md.nolink)
- [`ios_base`](../ios_base.md)`::`[`~ios_base`](op_destructor.md.nolink)
- [`ios_base`](../ios_base.md)`::`[`imbue`](imbue.md.nolink)
- [`ios_base`](../ios_base.md)`::`[`getloc`](getloc.md.nolink)
- [`basic_ios`](../basic_ios.md)`::`[`copyfmt`](../basic_ios/copyfmt.md.nolink)
- [`basic_ios`](../basic_ios.md)`::`[`exceptions`](../basic_ios/exceptions.md.nolink)