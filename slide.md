### Ruby Hack Challenge Holiday #9
- - -

### Numbered parameter の注意点

---

#### 自己紹介
- - -

* なまえ  : おしょー
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* Rails 歴 2年弱             <!-- .element: class="fragment" -->
* 最近のトレンド             <!-- .element: class="fragment" -->
  * [ピュア Ruby で Ruby 2.7 の Numbered parameter を実装してみよう！ - Secret Garden(Instrumental)](http://secret-garden.hatenablog.com/entry/2019/12/01/154607)   <!-- .element: class="fragment" -->
  * [一人 Ruby Advent Calendar 2017 - Qiita](https://qiita.com/advent-calendar/2017/ruby_pink_bangbi)                        <!-- .element: class="fragment" -->

---

### Numbered parameter の注意点

---

#### Numbered parameter とは
- - -

* ブロックの仮引数を省略して参照できる記法          <!-- .element: class="fragment" -->
* 第一引数を _1, 第二引数を _2, ...という記号で参照できる          <!-- .element: class="fragment" -->
* 略してナンパラ      <!-- .element: class="fragment" -->

>>>

```ruby
# 今までは仮引数を書く必要があった
[1, 2, 3].map { |it| it.to_s + it.to_s }
# => ["11", "22", "33"]
```

```ruby
# ナンパラを使うと
[1, 2, 3].map { _1.to_s + _1.to_s }
# => ["11", "22", "33"]
```
<!-- .element: class="fragment" -->

```ruby
# これは仮引数を定義しているのとだいたい同じ
[1, 2, 3].map { |_1| _1.to_s + _1.to_s }
```
<!-- .element: class="fragment" -->

>>>

```ruby
%w(homu mami).map(&:upcase)
%w(homu mami).each(&method(:puts))
%w(homu mami).map(&"name is ".method(:+))
```

```ruby
%w(homu mami).map { _1.upcase }
%w(homu mami).each { puts _1 }
%w(homu mami).map { "name is " + _1 }
```
<!-- .element: class="fragment" -->
## ナンパラを使うことでより簡潔に書ける！
<!-- .element: class="fragment" -->

---

## ナンパラを使ったときの注意点

---


## 1. 配列を受け取った時の挙動

---

#### 1. 配列を受け取った時の挙動
- - -

* 配列を渡した時は仮引数を定義した時と同じ                   <!-- .element: class="fragment" -->

```ruby
# 引数が1つの時はそのまま配列を受け取る
proc { |a| [a] }.call [1, 2] # => [[1, 2]]
# 引数が1つの時は配列を展開して受け取る
proc { |a, b| [a, b] }.call [1, 2] # => [1, 2]
```
<!-- .element: class="fragment" -->

```ruby
# ナンパラも同じ
proc { [_1] }.call [1, 2] # => [[1, 2]]
proc { [_1, _2] }.call [1, 2] # => [1, 2]
```
<!-- .element: class="fragment" -->

* _2 を使ったか使ってないかで _1 の意味が変わる
<!-- .element: class="fragment" -->


>>>

```ruby
# _1 だけ
proc { _1 }.call [1, 2] # => [1, 2]
```
<!-- .element: class="fragment" -->

```ruby
# _2 も使用した場合
proc { _2; _1 }.call [1, 2] # => 1
```
<!-- .element: class="fragment" -->

```ruby
user = { id: 1, name: "homu", age: 14 }
```
<!-- .element: class="fragment" -->

```ruby
user.map { _1 }
# => [[:id, 1], [:name, "homu"], [:age, 14]]
```
<!-- .element: class="fragment" -->

```ruby
# _2 を使用していると _1 = key になる
user.map { _2; _1 }
# => [:id, :name, :age]
```
<!-- .element: class="fragment" -->

---

## 2. `_1` 変数との併用

---

* Q. 以下は何が出力される

```ruby
_1 = :local_variable
p proc {
  _1
  proc {
    eval("_1")
  }.call "B"
}.call "A"
```

1. :local_variable            <!-- .element: class="fragment" -->
2. "A"           <!-- .element: class="fragment" -->
3. "B"           <!-- .element: class="fragment" -->
4. 構文エラー           <!-- .element: class="fragment" -->

>>>

## 答えは
## 3. :local_variable                        <!-- .element: class="fragment" -->
# 🤔                        <!-- .element: class="fragment" -->
## ナンパラなにもわからない                 <!-- .element: class="fragment" -->

>>>

#### _1 という名前で変数を定義した場合
- - -

* ナンパラよりも変数を優先する             <!-- .element: class="fragment" -->

```ruby
# _1 という名前のローカル変数が定義できる
_1 = :local_variable

# この場合はナンパラではなくてローカル変数を返す
proc { _1 }.call 42
# => :local_variable
```
<!-- .element: class="fragment" -->

---


## 3. _1 と binding.irb の組み合わせ

---

```ruby
proc {
  binding.irb
  _1
}.call 42
```


```ruby
irb> _1
=> 42
```
<!-- .element: class="fragment" -->

```ruby
irb> (1..10).map { _1 + _1 }
=> [84, 84, 84, 84, 84, 84, 84, 84, 84, 84]
```
<!-- .element: class="fragment" -->


## _1 を使ったコンテキストで irb を使用すると _1 が全て外の _1 を参照する

---

### まとめ
- - -

* ナンパラはめっちゃ便利                      <!-- .element: class="fragment" -->
* 普通に使う分には問題ない                    <!-- .element: class="fragment" -->
* ブロックが配列を受け取る場合に _1 _2 が何になるかを意識する必要はある    <!-- .element: class="fragment" -->
* _1 変数はナンパラよりも強い                     <!-- .element: class="fragment" -->
* _1 という名前の変数は定義しない！                     <!-- .element: class="fragment" -->

---

## ご清聴
## ありがとうございました
