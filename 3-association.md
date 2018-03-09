535c0ef0f1ace2a5
<!---- deploy info ---->
# アソシエーション：一対多

## 目的

```
アソシエーションの方法を学び、モデルとモデルを結びつけて情報を取得できるようになる。
```

## 今回のテーマ

```
一対多のアソシエーションを使えるようになりましょう。
```

Railsでは「アソシエーション」という機能を使用することにより、
「結びついている情報」をまとめて扱うことができます。
「アソシエーション」を理解するためには、まず **外部キー** について理解する必要があります。

※ここでは、本項までに作ったアプリケーションにテキストの機能を追加していく形で実装していきましょう。

## 外部キーについて

### 結びついているもの（日常編）

日常生活にある多くの物事は、「結びつき」があります。

[![https://diveintocode.gyazo.com/97379313a938699bc06ff17c86529104](https://t.gyazo.com/teams/diveintocode/97379313a938699bc06ff17c86529104.png)](https://diveintocode.gyazo.com/97379313a938699bc06ff17c86529104)

1. 学校と生徒
  「生徒が学校に属している」という結びつき
2. 会社と社員
  「社員が会社に属している」という結びつき etc...


### 結びついている情報（システム編）

先ほど例を上げた会社と社員について、テーブルとして書き起こしてみましょう。
この図のようになります。

[![https://diveintocode.gyazo.com/42868be3eef333674eec9fad938865d9](https://t.gyazo.com/teams/diveintocode/42868be3eef333674eec9fad938865d9.png)](https://diveintocode.gyazo.com/42868be3eef333674eec9fad938865d9)

ただこのままだと、2つのテーブルの「結びつき」を表すことができません。
ここではじめて、外部キーが必要になってきます。


### 外部キーとは
２つのテーブルを結びつけるときに使用する、テーブルのカラムです。

社員全員が「会社id」という「名札」をつけることにより、会社との結びつきを表す、というのが外部キーのイメージです。

[![https://diveintocode.gyazo.com/249fe2b848a8d51647b443ef870c3fc1](https://t.gyazo.com/teams/diveintocode/249fe2b848a8d51647b443ef870c3fc1.png)](https://diveintocode.gyazo.com/249fe2b848a8d51647b443ef870c3fc1)

### railsコマンドで外部キーのついたテーブルを作成

実際に上の例のように、外部キーの付いたテーブルを作成してみましょう。
まずは会社モデルを作成します。

```rb
## 会社モデルを作成
rails g model Company name:string
```

次に社員モデルを作成します。
社員は会社と結び付きがあるので、外部キーを持っている必要があります。
外部キーを持つテーブルは、モデル作成コマンドに`(結びつき先モデル名):references`を付け加えることにより作成可能です。  
今回の場合、以下のようになります。

```rb
rails g model Employee name:string company:references
```

ここで一旦、マイグレーションを実行しておきましょう。

```rb
rails db:migrate
```

では、社員と会社の結びつきを設定しましょう。
まずは、会社テーブルに"A会社"、"B会社"を追加します。
```rb
rails c
## 会社を追加
company = Company.new(name: "A会社")
company.save

company = Company.new(name: "B会社")
company.save
```

次に、社員を"A会社"に2名、"B会社"に1名追加します。
```rb
rails c
## 社員3人追加
employee = Employee.new(name: "太郎", company_id: 1)
employee.save

employee = Employee.new(name: "次郎", company_id: 1)
employee.save

employee = Employee.new(name: "三郎", company_id: 2)
employee.save
```

### ワーク
ここで、"A会社"に属する社員を全て取得するコマンドを実行してみましょう。

```rb
## A会社に属している社員を取得
Employee.where(company_id: 1)

## 発行されたSQL文
  Employee Load (0.3ms)  SELECT "employees".* FROM "employees" WHERE "employees"."company_id" = $1  [["company_id", 1]]

## 結果
=> [#<Employee:0x0055ff17856b48
  id: 1,
  name: "太郎",
  company_id: 1,
  created_at: Fri, 09 Mar 2018 06:20:10 UTC +00:00,
  updated_at: Fri, 09 Mar 2018 06:20:10 UTC +00:00>,
 #<Employee:0x0055ff178569e0
  id: 2,
  name: "次郎",
  company_id: 1,
  created_at: Fri, 09 Mar 2018 06:20:20 UTC +00:00,
  updated_at: Fri, 09 Mar 2018 06:20:20 UTC +00:00>]

```
このように扱うことが出来ます。


## アソシエーション

アソシエーションとは、結びつきのある複数のテーブルをひとまとめにして扱う事ができる、Railsの機能です。

これを使用すれば、Railsのコードをより直感的に、楽に書くことができます。


### アソシエーションの設定

では早速、実際にアソシエーションの設定をしていきましょう。

Railsでは、結びつけたいテーブルのモデルに対して、`has_many`や、 `belongs_to`を記述することで、アソシエーションを設定することが出来ます。

ここで、会社と社員の関係を見てみましょう。

1. 会社は社員をいっぱい持っている
2. 社員は一つの会社に属する

このような関係を「一対多」の関係、または「親と子の関係」と言います。

**会社は / 社員を / いっぱい持っている**
`app/models/company.rb`
```rb
class Company
   has_many :employees
end
```

**社員は / 一つの / 会社に属する**
`app/models/employee.rb`
```rb
class Employee
   belongs_to :company
end
```

`has_many`のときは複数形、
`belongs_to`のときは単数形になるところに注意してください。

このように設定をすることで、例えば、

```rb
rails c
company = Company.first ## firstはテーブル内の一番最初のレコードを取得するメソッド
company.employees ## アソシエーションで紐づいているレコードを取得するメソッド。モデルにアソシエーションの設定をすることで、アソシエーション名と同じ名前の関連情報を取得するメソッドが使えるようになります。
```

とすることで取得した会社のレコードにひもづく社員のレコードが全て取得できます。

さらに、

```rb
rails c
employee = Employee.first
employee.company
```

とすることで、社員が所属している会社のレコードを取得できます。ぜひご自身でも手を動かしてご確認ください。

### アソシエーションによるメリット

ここで、アソシエーションによるメリットをまとめてみましょう。

**1.  より直感的にコードを書くことができる**

先ほど、"A会社"の社員を取得するコマンドをかきました。
```rb
Employee.where(company_id: 1)
```

これをアソシエーションの設定後に書くと、以下のようになります。

```rb
## A会社に属している社員を取得
company = Company.find(1)
company.employees


=> 結果
<ActiveRecord::Associations::CollectionProxy [
<Employee id: 1, name: "太郎", company_id: 1, created_at: "2017-07-17 09:21:36", updated_at: "2017-07-17 09:21:36">,
<Employee id: 2, name: "次郎", company_id: 1, created_at: "2017-07-17 09:25:09", updated_at: "2017-07-17 09:25:09">
]>
```

**2. 外部キーが勝手に入る**

A会社に社員を追加するとき、以下のように書けます。

```rb
## A会社に属している社員を取得
company = Company.find(1)
company.employees.build({:name => "四郎"})

=>結果
<Employee id: nil, name: "四郎", company_id: 1, created_at: nil, updated_at: nil>
```

先ほどは、新しく社員を追加する際、
外部キーとして会社idを指定する必要がありましたが、
アソシエーションを用いるとその記載をする必要がなくなり、見通しが良くなります。


```rb
## 社員追加
employee = Employee.new({:name=> "太郎", :company_id=> 1})
employee.save
```


## まとめ

```
結びつきのある複数のテーブルをひとまとめにして扱う事ができる、Railsの機能として、アソシエーションがある。
アソシエーションを実装するためには、外部キーを片方のテーブルに設定する必要がある。
Railsでは、結びつけたいテーブルのモデルに対して、has_many, belongs_toを記述することで、アソシエーションを設定することが出来る。
```

### 小課題1

小課題は課題として提出する必要はありませんが、必ず一度手を動かして取り組んでみてください！（プログラミングは手を動かす経験がとても大切です。課題に対する質問や解答の確認がしたい場合は、ぜひ質問投稿にご投稿ください）

テキストで出た会社と社員の例で、

```
company.destroy
```
を行ったときにその会社に属する社員も消えるようにするには、
どうすればよいかを解答してください。（一対多のアソシエーションで、一の側が消去された時、紐づいている多の側も一緒に削除されるようにしたいです。答えは一番下に記述してありますが、一度自分で検索して答えを探して見ましょう）

### 小課題2

user機能とblog機能を持ち、それらが一対多で紐づいているアプリケーションを作成してください。また、紐付けがあることがわかるように二つに適当なデータを挿入しておいてください。
（この課題は今まで作ったアプリに機能を拡張する形でも、一からアプリを作成する形でも構いません。提出の必要はありませんが、次のカリキュラムはこのアプリを使用して進めていくので、必ず実装をお願いいたします）

## お疲れ様でした

この章では、複数のモデル間で情報のやりとりができるアソシエーションについて解説して行きます。

アソシエーションはアプリケーションを作ろうとした時に必ず関わる技術ですので、ぜひここで使い方を覚えていきましょう。

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

```rb
class Company < ActiveRecord::Base
    has_many :employees, dependent: :destroy
end
```

このように、`一`側のモデルに書いてある`has_many`の部分に`dependent: :destroy`というオプションを付け加えることで、一側のレコードが消えた時に、ひもづく多側のレコードも削除されます。

`rails 一対多 destroy`
`rails association dependent`

などとググると出てきます。ググる力もエンジニアにはとても大切なものなので、わからないことがあったらググる癖を身につけておきましょう。
