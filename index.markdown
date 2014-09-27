# tokushima.rb 1 slide

### Author

* か (ka)
* [kaosfield](http://www.kaosfield.net)


## Ruby入門

今日は記念すべきtokushima.rbの第1回なので発表します

内容も簡単なものにします

今回説明に使うコードは全て [kaosf/20140831-tokushimarb-codes](https://github.com/kaosf/20140831-tokushimarb-codes) に置いておきます

このスライド自体のリポジトリは [kaosf/20140831-tokushimarb-slide](https://github.com/kaosf/20140831-tokushimarb-slide) にあります

間違いを発見したら Issue あるいは PullRequest を下さい


## Rubyの特徴

Ruby on Railsが使える！ワーイ


## それは2014年になった今の話

確かにこれが大きな特徴の一つになってしまっているがRails以前のRubyでは恐らくRubyの特徴と言えば次の2点であったはずである


## Rubyの特徴(Rails以前)

* 全てがオブジェクトである
* 作者が日本人である


## 90年代から2000年代初頭頃の話

世は正に大オブジェクト指向時代

後に30億のデバイスで動くことになる言語や主にCGIを作成するためのシェルスクリプトに毛が生えたような言語が幅をきかせていた

そんな中で「日本人が作った」「全てがオブジェクトである」という面白い特徴を備えた言語が産まれてきた

それがRubyである


## 全てがオブジェクトになる

プリミティブ型というものは存在しない．

たとえば `1` という整数値一つとってもそれはオブジェクトでありメソッドを備えており，そしてクラスに属している．


## 確認

irbを起動して，

```ruby
1.methods
1.class #=> Fixnum

1.instance_of? Fixnum #=> true
```

等としてみる．そもそもこれが出来る時点で1はオブジェクトであると言える．1は`Fixnum`クラスのオブジェクトなのである．


## おもしろメソッド一部紹介

* `methods`: 全メソッドが配列になって返ってくるメソッド
* `class`: インスタンスのクラスが返ってくるメソッド
* `instance_variables`: インスタンス変数(他の言語でのメンバ変数やプロパティと考えて良い)の一覧が配列になって返ってくるメソッド


## instance_variables例

```ruby
class A
  def initialize
    @x = 1
    @y = 2
  end
end

p A.new.instance_variables
#=> [:@x, :@y]
```


## Fixnumならではのメソッド一部紹介

* `+`: `1.+(2)`と引数を渡してやればお察しの通り`3`が返ってくるメソッド
* `to_s`: to stringの略で`1.to_s`で`"1"`という文字列が返ってくるメソッド


## 補足

ここで言う「オブジェクト指向」は，IoやJavaScript等に見られる「プロトタイプベース」のものではなく，C++やJavaに見られる「クラスベース」のものであるとする．

後「インスタンス」と「オブジェクト」の用語の使い分けは特に意識しない．


## Rubyが徹底している点

Rubyでは「全て」がオブジェクトである．例外は無い．


## Javaでは

プリミティブ型を除き全てのクラスは一番基底のクラスに`Object`クラスを持っている．

なので「全てのインスタンス is a Object」と言える．

しかしこれは「クラス」という概念そのものは何かのクラスに属しているということではないと言える．

当たり前に聞こえるだろうか？


## Rubyでは

Rubyではクラスという概念すら，オブジェクトである．

Rubyには`Class`クラスが存在する．全てのクラスは，`Class`クラスのオブジェクトなのである．


## 確認

```ruby
1.class #=> Fixnum
Fixnum.class #=> Class
```


## ここで自然に一つ気になることが

`Class`クラスは何と言うクラスのオブジェクトなのか？


## お察しの通り

当然`Class`クラスも`Class`クラスのオブジェクトである．以下のコードで確認出来る

```ruby
Class.class #=> Class
```


## 謎の存在

公式ドキュメントを少し読んでみる

* [library _builtin](http://docs.ruby-lang.org/ja/2.1.0/library/_builtin.html)
* [class Class](http://docs.ruby-lang.org/ja/2.1.0/class/Class.html)

> クラスのクラスです。
> 
> より正確に言えば、個々のクラスはそれぞれメタクラスと呼 ばれる名前のないクラスをクラスとして持っていて、Class はそのメタ クラスのクラスです。この関係は少し複雑ですが、Ruby を利用するにあたっ ては特に重要ではありません。


## メタクラス

[メタクラス完全理解 | yohasebe.com](http://yohasebe.com/wp/translation/%E3%83%A1%E3%82%BF%E3%82%AF%E3%83%A9%E3%82%B9%E5%AE%8C%E5%85%A8%E7%90%86%E8%A7%A3)

ここの記事を紹介するに留める


## 手続き脳の皆さんfor文を捨てよう

```ruby
for i in [1, 2, 3] do
  puts i + 1
end
```

ShellScirpt風味があって良いがRubyではほぼ100%以下のようにする

```ruby
[1, 2, 3].each do |i|
  puts i + 1
end
```


## 無名関数

ラムダ関数とも言う

配列というオブジェクトがeachという全要素を走査するメソッドを持っている

走査時に行う処理は決められていない

処理をその場で関数を作って指定してやる

別の言語ではforeachと言われることもある


## イメージ 1

```ruby
[1, 2, 3].each
## each「処理内容が分からんぽよ．誰か関数ちょうだい．」
```


## イメージ 2

```ruby
[1, 2, 3].each a_function
# 自分「a_function を渡してしんぜよう」
# each「a_function って中身どうなってんの？」
# 自分「メンゴメンゴ，内容は + 1 して出力して欲しいねん．具体的に内容後で書くわ．」
```


## イメージ 3

```ruby
[1, 2, 3].each do |i|
  puts i + 1
end
# each「言うてワンチャンもう処理内容ここで書いてくれてもええんちゃう？」
# 自分「ええで．do から end までが a_function 関数(処理)ね．引数iに要素を渡してね．」
# each「オッケー」
```


## 大雑把ではあるが…

おおよそこのような意図をもってして無名関数(※既に`a_function`という名前は無い)を指定して処理をしてやることが出来る

`do ... end` でこれが書けるというのがRubyの特徴の一つ

コードブロックあるいは単にブロックと言う


## コードブロックもオブジェクト

Rubyでは厳密に言うと「関数」すら存在しない

`Proc`というクラスのオブジェクトである


## コードブロックを受け取るメソッド

```ruby
def f(&block)
#def f # &block は最後の引数で確定しているので yield で呼ぶなら指定不要
  puts 'before'
  block.call
  #yield # block.call の代わりに yield でも呼べる
  puts 'after'
end

f do
  puts 'in the code block'
end
```


## クラス確認

```ruby
def f(&block)
  puts block.class
end

f do
end
#=> Proc
```


## Procクラスのインスタンス作成

```ruby
pr = Proc.new do
  puts "in the proc"
end

lm = lambda { puts "in the proc" }

puts pr.class #=> Proc
puts lm.class #=> Proc

pr.call #=> in the proc
pr[]    #=> in the proc
lm.call #=> in the proc
lm[]    #=> in the proc
```

これを作るのにも `do ... end` を使うの変な気はするが…

lambdaとして作ることも出来る


## 補足

```ruby
do
  code
end
```

と

```ruby
{ code }
```

に違いは無い

後者の方がワンライナーにしやすい程度

do end をワンライナーにするのは以下の通り

```ruby
do; code; end
```


## Procとlambdaの違い

引数の数のチェックの有無

```ruby
pr = Proc.new do |x|
  puts "x is #{x}."
end

lm = lambda { |x| puts "x is #{x}." }

pr[]
#lm[] #=> ERROR!
pr[1]
lm[1]
```


## Procとlambdaの違い

他にもある

[class Proc](http://docs.ruby-lang.org/ja/2.1.0/class/Proc.html)

> lambda と proc と Proc.new とイテレータの違い


## eachにProcインスタンスを作って明示的に渡す

```ruby
pr = Proc.new do |x|
  puts x + 1
end

[1, 2, 3].each(pr)
```

これではエラーになる


## 何故か？

コードブロックを引数として渡すのは通常の引数を渡すのと同じでは無い

最後の実引数として `&` を付けて「これはコードブロックです」と教えてやらなければならない

なのでエラーの内容は「0引数のeachメソッドに1引数が渡されている」である


## 解消する

```ruby
pr = Proc.new do |x|
  puts x + 1
end

[1, 2, 3].each(&pr)
#=> 2
#   3
#   4
```


## Rubyでは全てがObjectである

Rubyの最大の特徴はこれであると私は考えている

それを踏まえてRubyの勉強をしていくとメタプログラミングの闇に足を踏み入れる準備が出来ると思う

`method_missing`メソッド楽しいよ！


## 参考サイト

* [[Ruby] ブロックとProcをちゃんと理解する - Qiita](http://qiita.com/kidachi_/items/15cfee9ec66804c3afd2)
* [Rubyの動かないコード　（初級編）　ブロックとクロージャの性質 - 主に言語とシステム開発に関して](http://d.hatena.ne.jp/language_and_engineering/20101118/p1)


おわり
