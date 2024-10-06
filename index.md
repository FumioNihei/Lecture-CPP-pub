
<link rel="stylesheet" href="style.css">


# 実験で見つけたもろもろ

- [第3回 2024/09/27](#第3回)
- [第2回 2024/09/27](#第2回)

# 第3回

## デバッグのテクニック

デバッグ（プログラムのバグをなくす作業．）においてやるべきは，**エラー箇所の特定**と，**エラー内容の特定**，の二点であることがほとんど．特に長いソースコードを書くと，この二点の特定が難しくなる．

なんか長いコード（以下の例）を作るときには，まずはエラー箇所を特定するために，プログラムのほとんどをコメントアウトしコンパイル，その後コメントアウトの範囲を狭めてコンパイル，を繰り返すとよい．以下の例参照．


<div class="row"><div class="column">

一回目は，まず以下のように，ほとんどをコメントアウトしてコンパイル．

```cpp
#include <iostream>
#include <string> 

int main() {
  std::string s {"Hello!"};
  std::cout << s <<"\n";

  /*
  s = "How are you?";
  std::string s2 {s};
  std::string s3;
  s3 = s2;
  std::cout << s3 <<"\n";

  std::cin >> s2;
  s3 = s2 + "! ";
  s3 = s3 + s3;
  std::cout << s3 <<"\n";

  s2 += ", thank you,";
  s3 = " and You";
  s2 += s3;
  s2 += '?';
  std::cout << s2 <<"\n";
  */
  return 0;
}
```


</div><div class="column">

続けて二回目は，以下のように，コメントアウトの範囲を狭めてコンパイル．

```cpp
#include <iostream>
#include <string> 

int main() {
  std::string s {"Hello!"};
  std::cout << s <<"\n";

  s = "How are you?";
  std::string s2 {s};
  std::string s3;
  s3 = s2;
  std::cout << s3 <<"\n";

  /* 
  std::cin >> s2;
  s3 = s2 + "! ";
  s3 = s3 + s3;
  std::cout << s3 <<"\n";

  s2 += ", thank you,";
  s3 = " and You";
  s2 += s3;
  s2 += '?';
  std::cout << s2 <<"\n";
  */
  return 0;
}
```

</div></div>

c++では，`/* */` で囲まれた範囲は，一括でコメントアウトできる．複数行を一括でコメントアウトしたいときに便利．

また，ソースコードを全て作成してからコンパイルするのではなく，**少しずつ書き進めながら逐次コンパイルするほうが，慣れないうちは効率がよい**ことがほとんど．

## コンパイラのエラーの読み方

以下のようなコンパイラのエラーや，もっと長いものに遭遇した人もいるはず．

<pre>
$ g++ -std=c++17 student.cpp
student.cpp: In function ‘int main()’:
student.cpp:6:5: error: ‘string’ was not declared in this scope
    6 |     string s {"hello"};
      |     ^~~~~~
student.cpp:6:5: note: suggested alternatives:
In file included from /usr/include/c++/11/iosfwd:39,
                 from /usr/include/c++/11/ios:38,
                 from /usr/include/c++/11/ostream:38,
                 from /usr/include/c++/11/iostream:39,
                 from student.cpp:1:
/usr/include/c++/11/bits/stringfwd.h:79:33: note:   ‘std::string’
   79 |   typedef basic_string<char>    string;
      |                                 ^~~~~~
In file included from /usr/include/c++/11/bits/locale_classes.h:40,
                 from /usr/include/c++/11/bits/ios_base.h:41,
                 from /usr/include/c++/11/ios:42,
                 from /usr/include/c++/11/ostream:38,
                 from /usr/include/c++/11/iostream:39,
                 from student.cpp:1:
/usr/include/c++/11/string:67:11: note:   ‘std::pmr::string’
   67 |     using string    = basic_string<char>;
      |           ^~~~~~
</pre>

コンパイラのエラーは，たいていわかりにくい．そのため，プログラムが書ける人も，コンパイラのエラーを読むのは渋々，という人が多い．渋々読まなければならない時には，コンパイラのエラーを全て理解しようとするのではなく，一番最初のエラーだけ確認すれば，まずはOK．この例における一番最初のエラーとは，以下の箇所である．

<pre>
student.cpp: In function ‘int main()’:
student.cpp:6:5: error: ‘string’ was not declared in this scope
    6 |     string s {"hello"};
      |     ^~~~~~
</pre>

この例では，メイン関数の中において `In function ‘int main()’`，6行目の5文字目 `6:5` にエラーがあることを示している．具体的なエラーは `‘string’ was not declared in this scope` と書かれているが，この辺りは参考程度にしたうえで，自身が書いたソースコードを見直していくとよい．**コンパイラはあくまで構文のエラーを示すだけであって，どう直せば動く，とは教えてくれない**ことに注意．

# 第2回

## インデント

**インデント**：行頭に書いておく，連続したスペースのこと．字下げ．

c++では，インデントは任意．中括弧開き `{` の後の行では，インデントをしておくと見やすいコードができる．そもそもc++では，連続したホワイトスペース（スペース` `, 改行`\n`, タブ文字`\t`, CR改行`\r`）は無視されてコンパイルされる．

pythonでは，インデントでブロック（処理のまとまり）を示すため，インデントは必須．

pythonではインデントの有無によって処理が変わってしまうが，c++ではインデントがいくらあっても処理は変わらないため，インデントはいくら入れてもOK．

<div class="row"><div class="column">
    
### c++

```cpp
#include <iostream>

int main() {
    // main関数の開始で中括弧が開いた { ので，インデント
    std::cout << "Hello\n";
    
    int x{5};
    if( x >= 0 ) {
        // if 文で中括弧が開いた { ので，インデント
        std::cout << "x is positive value\n";
    }
    
    // if 文の中括弧が閉じた } のでインデントを戻す
    std::cout << "if statement is finished\n";

    // 中括弧を開かないのであれば，インデントは変更なし
    if( x >= 100 ) std::cout << "x is large value\n";
}

// main関数の終了で中括弧が閉じた } のでインデントを戻す
```


</div><div class="column">

### python

```python


if __name__ == "__main__":
    # python のmain関数（相当）の開始のため，インデント
    print( "Hello" )

    x = 5
    if x >= 0:
        # pythonはインデントでブロックが決まる
        print( "x is positive value" )


    # if 文から抜けたことを示すため，インデントを戻す
    print( "if statement is finished" )

    # 処理が一つだけなら，インデントを下げなくてもOK
    if x >= 100: print( "x is large value" )

```

</div></div>


## c++のホワイトスペースの扱い

極端な話をすれば，以下のコードでもコンパイルがかかる．相当自由に改行や空白を入れられる．しかしこんなコードは書いてはいけない．一方，`#include <iostream>`の間には改行は入れるとコンパイルエラーになった．

```cpp
#include         <iostream>

int


main


      (
)

{
          std


::


     cout <<


"kuso spacing\n";
            }
```

## セミコロン `;` とコロン `:`

c++では，処理の区切りのタイミングで，セミコロン `;` を使用する．[c++でのホワイトスペースの扱い](#cのホワイトスペースの扱い)で示したように，c++ではかなり自由に空白や改行を挿入することができるため，セミコロン `;` を一つの区切りとしている．

pythonでは，改行で処理が区切られる（ `()`を使えば，改行を挟んでも処理を継続できるが，それは例外 ）．そのため， `;` で処理を区切るc++は，pythonとは根本的に違うものだと理解しておく必要がある．

また，（当然だが）pythonにおける `:` と，c++における `;` は，役割が全く異なるため，同じように使ってはいけない．例えば以下のコード．


<div class="row"><div class="column">
    
### python

```python
x = 10
if x >= 0:
    print( "x is positive" )
```

</div><div class="column">

### c++

```cpp
int x{10};
if ( x >= 0 );
    std::cout << "x is positive";
```

※ main関数などは省略

</div></div>


pythonでの実装を基準に，コロン `:` をセミコロン `;` に変更した．c++のソースコードに着目すると，`x >= 0`のため，if文の条件を満たすが，if文が直後で `;` により処理が区切られたため，**何もしない処理をすることになる**．さらに，インデントで惑わされやすいが，**続く`std::cout`はif文に関係なく必ず実行される**．これは意図した動作ではないため，気を付けること．


## 複数個の値をキーボード入力するときのプログラム

```cpp
#include <iostream>

int main() {
    int x{}, y{}, z{};

    std::cin >> x >> y >> z;

    // 良くありがちなミス
    // std::cin << x << y << z;
    // std::cin >> x, y, z;
    // std::cin >> x >> y >> x >> "\n";
}
```

`std::cin`に続く演算子の向き `>>` と，変数ごとに `>>` で区切ることに注意．

また，`cin` での入力後に．画面に改行を表示したい場合は，`cin >> \n;` とするのではなく，`cin` の次の行で，`cout << "\n";` とする必要がある．

## 作成したプログラムに，複数個の値をキーボード入力するときのやり方

キーボードのスペースを入力するか，エンターキーを入力して，複数個の値を区切ればOK（値はホワイトスペースで区切って入力する）．

```cpp
#include <iostream>

int main() {
    int x{}, y{}, z{};
    std::cin >> x >> y >> z;
    std::cout << x << ", " << y << ", " << z << "\n";
}
```

<pre>
$ g++ -std=c++17 sample.cpp -o sample.out
$ ./sample.out   # スペースで複数個の入力を区切る場合
3 5 7            # 入力部分
3, 5, 7          # ここが出力部分
$ ./sample.out   # 改行で複数個の入力を区切る場合
3                # 入力部分1
5                # 入力部分2
7                # 入力部分3
3, 5, 7          # ここが出力部分
</pre>


## 複数個の値を画面に出力するとき

```cpp
#include <iostream>

int main() {
    int x{0};
    char ch{ 'a' };

    std::cout << "x is " << x << ", ch is " << ch << "\n";
    // ↓ 出力
    // x is 0, ch is a + 改行

    // 改行やスペースで見やすくしておくとよいかも
    std::cout <<   "x is " << x
              << ", ch is " << ch
              << "\n";

    // 良くありがちなミス．改行に注目．
    // std::cout << "x is " << x << ", ch is " << ch << \n;
}
```

改行を示す `\n` は，あくまでも文字という扱いなので，（今後勉強する）文字列型として表現する `"\n"` か，文字型 `'\n'` として表現する．ダブルクオーテーション `"` かシングルクオーテーション `'` で括る必要がある．


## 課題関係：どこまでが文字列かの判断をする

**文字列**：ダブルクオーテーション `"` で括られた値．

以下は，ある課題の実行結果とする．**下線部は入力**である，という記載がある場合．

<pre>
$ ./sample.out
Input three integers: <u>30</u> <u>50</u> <u>10</u>
50 > 30 > 10
</pre>

実行結果から，どこが出力かを推測すること．この例では，`Input three integers:` が出力．コロン `:` などの記号には，何となく特別な意味を見出してしまうが，これは文字列としてのコロン `":"` であり，特別な意味は一切ない．

`50 > 30 > 10` を出力する際にも，不等号 `>` にはなんとなく特別な意味を見出してしまうが，これも文字列としてのコロン `">"` であり，特別な意味は一切ない．

下線が引かれている `30 50 10` については，キーボード入力をしている最中の様子が，画面上に表示されているだけなので，これは出力ではないことに注意．以下のソースコード参照．


```cpp
#include <iostream>

int main() {
    // 正しい
    int x, y, z;
    std::cout << "Input three integers:";
    std::cin >> x >> y >> z;  // ここでユーザが入力している最中の様子が，画面に表示されるので，「下線部は入力」の要件を満たす．
    
    // // 以下はありがちな間違い
    // std::cout << "Input three integers:";
    // std::cin >> x >> y >> z;
    // std::cout << x << " " << y << " " << z << "\n";
    // // このようにすると，ユーザの入力の様子が画面に表示された後，再度coutで入力された値を表示することになり，冗長かつ不要．
}
```

ありがちな間違いを実行すると，以下のようになる．

<pre>
$ ./sample.out
Input three integers: 30 50 10
30 50 10         # ここが冗長かつ不要なポイント
</pre>

## else

ifとelse ifは条件を持つが，elseは条件を持たない．elseに対して条件を持たせようとすると，その後に続く式の構文に誤りが生じ，シンタックスエラーによりコンパイルができなくなることが一般的なため，実装していればelseに条件を持たせることは，おおむね回避できる．

しかし，いくつかの不幸が重なると，シンタックスエラーが生じず，コンパイルが通ってしまうことがある．以下のコード参照．

```cpp
#include <iostream>

int main() {
    int x {0};
    
    if ( x == 0 )
        std::cout << "x is zero.";
    else ( x != 0 );
        std::cout << "x is not zero.";
}
```

まるでelseが条件を持っているように見えてしまう．しかし正しい解釈は，**elseに落ちたときに，式 `( x != 0 )` を実行する**，である．インデントで混乱させられるが，`std::cout << "x is not zero.";`はif else文に関係なく，必ず実行される．つまり，`x == 0`のときの出力は，`x is zero.x is not zero.`である．
