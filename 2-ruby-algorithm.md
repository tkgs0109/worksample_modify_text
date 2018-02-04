# Rubyアルゴリズム・データ構造3

## 目的

```
線形探索法（リニアサーチ）の実装ができるようになる。
```

## 今回のテーマ

```
手を動かしながら、アルゴリズムの実装を学んでいきましょう。
```

## 線形探索法（リニアサーチ）

線形探索法は、リストや配列に入ったデータを先頭から順に調べていく検索のアルゴリズムの１つです。

### 線形探索法アルゴリズムの流れ

線形検索法の考え方はいたってシンプルです。下記の図のような配列があるとします。

<a href="https://diveintocode.gyazo.com/2e32e72d36ffbedc3fea5985c2d39175"><img src="https://t.gyazo.com/teams/diveintocode/2e32e72d36ffbedc3fea5985c2d39175.png" alt="https://diveintocode.gyazo.com/2e32e72d36ffbedc3fea5985c2d39175" width="309"/></a>

このデータ`5`がどこにあるか検索したいときの流れは

1. 最初のデータである1を見ます。

<a href="https://diveintocode.gyazo.com/03b22ba806d6f25d6af0b71c236048ad"><img src="https://t.gyazo.com/teams/diveintocode/03b22ba806d6f25d6af0b71c236048ad.png" alt="https://diveintocode.gyazo.com/03b22ba806d6f25d6af0b71c236048ad" width="309"/></a>

2. 1ではないので、次のデータ3を見ます。

<a href="https://diveintocode.gyazo.com/8067e882d32958251f6d7984b162edcd"><img src="https://t.gyazo.com/teams/diveintocode/8067e882d32958251f6d7984b162edcd.png" alt="https://diveintocode.gyazo.com/8067e882d32958251f6d7984b162edcd" width="309"/></a>

3. 3ではないので、次のデータ5を見ます。5を見つけたので処理を終了させます。

<a href="https://diveintocode.gyazo.com/729db5a89bf7d1f67308d670157b590e"><img src="https://t.gyazo.com/teams/diveintocode/729db5a89bf7d1f67308d670157b590e.png" alt="https://diveintocode.gyazo.com/729db5a89bf7d1f67308d670157b590e" width="309"/></a>

つまり配列の最初から順番に探していくのです。

線形探索はもっとも簡単なアルゴリズムのひとつと言えますが、このアルゴリズムを使用する場合、

最大でリストの要素数と同じ回数の探索が必要になる場合があります。

例えば、12を発見するためには合計5回の試行が必要です。


## 二分探索法（バイナリサーチ）

二分探索法は、探索の対象となるデータが、
あらかじめ小さい順または大きい順に整列されている場合に使うことができる探索アルゴリズムです。

### 二分探索法アルゴリズムのイメージ

まずは、イラストを用いて二分探索アルゴリズムのイメージを掴みましょう。

[![https://diveintocode.gyazo.com/3ae90b2e79618a18353b498aad5807e5](https://t.gyazo.com/teams/diveintocode/3ae90b2e79618a18353b498aad5807e5.png)](https://diveintocode.gyazo.com/3ae90b2e79618a18353b498aad5807e5)

今回は「5」という数字を検索する場合を想定してみましょう。

まず、最初に配列の中央の値に注目します。

この場合、配列の中央の値は「4」になります。

[![https://diveintocode.gyazo.com/9c605638a6f4e70c29cbe17d196ccd51](https://t.gyazo.com/teams/diveintocode/9c605638a6f4e70c29cbe17d196ccd51.png)](https://diveintocode.gyazo.com/9c605638a6f4e70c29cbe17d196ccd51)

中央の値である「4」と今回検索対象としている「5」を比較すると「5」の方が大きいです。

このことから「5」は「4」の右側にあることがわかります。

[![https://diveintocode.gyazo.com/5bf14c9936e96227523e2a865f7ee122](https://t.gyazo.com/teams/diveintocode/5bf14c9936e96227523e2a865f7ee122.png)](https://diveintocode.gyazo.com/5bf14c9936e96227523e2a865f7ee122)

再び、配列の中央の値に注目します。

この場合、配列の中央の値は「6」になります。

[![https://diveintocode.gyazo.com/5fff9011b259acd8c8c24a877b69f6f0](https://t.gyazo.com/teams/diveintocode/5fff9011b259acd8c8c24a877b69f6f0.png)](https://diveintocode.gyazo.com/5fff9011b259acd8c8c24a877b69f6f0)

中央の値である「6」と今回検索対象としている「5」を比較すると「6」の方が大きいです。

このことから「5」は「6」の左側にあることがわかります。

このように、探索する範囲を2つに分けて、半分に絞込みながら探索を進めていくのが「二分探索法」です。

### 二分探索法の処理

二分探索法は、大きく分けて以下の3つの処理から構成されています。

- 中央の要素を選ぶ
- 中央の要素と、検索対象の要素を比較
- 探索の範囲を半分に狭める

**中央の要素を選ぶ**

先ほどのイラストに`添え字`を追加すると次のようになります。

[![https://diveintocode.gyazo.com/267607b42f8f8852f924c64bad78764d](https://t.gyazo.com/teams/diveintocode/267607b42f8f8852f924c64bad78764d.png)](https://diveintocode.gyazo.com/267607b42f8f8852f924c64bad78764d)

配列の中央の数字を計算で出す場合は、
`( 配列の先頭の添字 + 配列の末尾の添字 ) / 2`という計算式で導くことが出来ます。
今回の場合、`( 0 + 6 ) / 2 = 3`となり、中央の値の添字は[3]となります。

**配列の要素の数が偶数の場合**

要素の数が5個や7個の場合は2で割り切ることが出来るのですが、

例えば要素の数が6個の場合、配列の中央を求める計算式は、`0+5/2=2.5`となります。

添字[2.5]という要素は存在しないため、小数を整数化する必要があります。

小数を整数化する方法としては、「四捨五入」、「切り上げ」、「切り捨て」といった方法が考えられます。

**中央の要素と、検索対象の要素を比較**

中央のデータを決定したら、次はそのデータと、検索対象のデータが一致しているかどうか比較します。

中央のデータが検索対象のデータと一致した場合、検索対象のデータが見つかったことになり、その時点で探索は終了になります。

中央のデータが検索対象のデータと一致しない場合、
中央のデータが検索対象のデータよりも、大きいか、小さいかの2パターンが考えられます。

**探索の範囲を半分に狭める**

先程の比較結果をもとに、探索範囲を絞って行きます。

|比較結果|絞り込み方|
|:---------------|:----------|
|検索対象のデータ<中央のデータ|前半分に絞る|
|中央のデータ＜検索対象のデータ|後ろ半分に絞る|

二分探索法では、上記の処理を繰り返し行うことで対象のデータを検索します。

**検索対象のデータが存在しない場合**

先頭の要素の値が末尾の値よりも大きくなった場合、検索対象のデータが存在しないということが言えます。

検索対象のデータが存在しない場合に、無限ループに入ってしまわないよう、

中央のデータを選ぶ処理を実行する前に、先頭の要素の値が、末尾の値以下であることを確認する条件式が必要です。

### 二分探索法のアルゴリズム

二分探索法のここまでの処理をアルゴリズムとして表すと以下のようになります。

変数はそれぞれ次のものを表しています。

|変数   |対象         |
|:-----|:------------|
|head  |先頭の要素の添字|
|tail  |末尾の要素の添字|
|center|中央の要素の添字|

[![https://diveintocode.gyazo.com/23af87d34c6b3fe559eb5b7de4527596](https://t.gyazo.com/teams/diveintocode/23af87d34c6b3fe559eb5b7de4527596.png)](https://diveintocode.gyazo.com/23af87d34c6b3fe559eb5b7de4527596)

## まとめ

```
線形探索法のアルゴリズムは、配列に保存されたデータを先頭から順番に探索し、
探索処理は反復構造で記述する。
二分探索法のアルゴリズムは、前提として、検索対象のデータが昇順か降順に
整列され、探索する範囲を半分に絞りながら探索する。
```

## 小課題1

小課題は課題として提出する必要はありませんが、必ず一度手を動かして取り組んでみてください！（プログラミングは手を動かす経験がとても大切です。課題に対する質問や解答の確認がしたい場合は、ぜひ質問投稿にご投稿ください）

カリキュラムを見返しながら、線形探索法のアルゴリズムをRubyを使って実装してください。
（正解のコードの例は、このカリキュラムの最後に載せていますが、その前に自分で実装してみてください。もしうまくできなかったら、正解のコードと自分の書いたコードを見比べて、何が足りなかったかを復習してください）

以下のコードを利用し、`classic_algorithm.rb`を作成してください。

`classic_algorithm.rb`

```ruby
# 以下に線形探索法を行う関数を定義してください
def linear_search(numbers,value)
  # この部分を記述してください
end

numbers = [1,3,5,11,12,13,17,22,25,28]
p(numbers)

puts('探したい数字を入力してください')
target_number = gets.to_i

message = linear_search(numbers,target_number)

puts(message)

```

### 実装要件

1. 探したい数字を入力できること
2. 入力した数字がもしあれば、数字が存在するインデックスを表示させること
3. もし探したい数字がなければ`None`と表示させること
4. インデックスは0から考え、インデックス0に目的の数字が合った場合、`0`と出力させること

例

```
$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 1
>> 0

$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 13
>> 5

$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 2
>> None
```

## 小課題2

二分検索法のアルゴリズムをRubyを使って実装してみましょう。こちらも正解のコード例は下に載せています。


以下のコードを利用し、`classic_algorithm2.rb`作成してください。

`classic_algorithm2.rb`

```ruby

# 以下に二分探索法を行う関数を定義してください
def binary_search(numbers,value)
  # この部分に記述してください
end

numbers = [1,3,5,11,12,13,17,22,25,28]
p(numbers)

puts('探したい数字を入力してください')
target_number = gets.to_i

message = binary_search(numbers,target_number)

puts(message)
```

### 実装要件

1. 探したい数字を入力できること
2. 入力した数字がもしあれば、数字が存在するインデックスを表示させること（例 3）
3. もし探したい数字がなければ`None`と表示させること
4. インデックスは0から考え、インデックス0に目的の数字が合った場合、`0`と出力させること

## お疲れ様でした。

線形探索法と二分探索法を比較すると、一般的には二分探索法の方が探索速度が早いとされています。

しかし、データが少ない場合や、目的のデータが先頭に近い位置に存在する場合、線形探索法の方が実行速度が早くなります。

アルゴリズムはデータ量や格納状況、整列状態に応じて適切なものを選択することが重要です。

質問はこちらのURLにお願いいたします。

https://diveintocode.jp/diver/questions/new

## テキストフィードバックについて

今後のDIVE INTO CODEの発展のために、お時間があれば
フィードバックのご協力をお願い致します。
以下を参考に、DIVERのコメント欄にご投稿して頂けると有難いです。

・テキストの難易度は適切であったか

・テキストの分量は適切であったか

・説明していない用語はなかったか

・テキストに間違いはなかったか

・テキストのゴールに対して適切な内容であったか

・他に説明を加えてほしいことは無かったか

・やってて楽しいテキストであったか

・有益なテキストであったか

**その他なんでもご意見をご投稿ください**

**DIVER質問機能のコメント欄に記述をお願い致します。**

**小課題1の解答例**

```
def linear_search(numbers,value)
  i = 0
  while i < numbers.length
    if numbers[i] == value
      return i
    end
    i+=1
  end
  return "None"
end
```

**小課題2の解答例**

```
def binary_search(numbers,value)
  head = 0
  tail = numbers.length

  while head <= tail do
    center = ((head + tail) / 2).round
    if (numbers[center] == value)
      return center
    elsif (numbers[center] < value)
      head = center + 1
    else
      tail = center - 1
    end
  end
  return "None"
end
```