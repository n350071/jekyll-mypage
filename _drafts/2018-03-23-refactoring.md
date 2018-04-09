---
layout: single
title:  "RSpecはじめたばかりだけど、サービスクラスを使いたい"
categories: EscapeSurfing develop
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

## modelのテストがしずらい

### なにが原因？
このメソッドのテストをしたい。 `gem factory_bot_rails`を入れてフィクスチャを使ってテストしようとしています。
しかし、このメソッドに渡す`auth_hash`は`User`と微妙に違って、ハッシュの入れ子になっています。
これがテストのしにくさになっています。

{% highlight ruby linenos %}
def self.find_or_create_from_auth_hash(auth_hash)
  provider = auth_hash[:provider]
  uid      = auth_hash[:uid]
  nickname = auth_hash[:info][:nickname]
  image    = auth_hash[:info][:image]

  User.find_or_create_by(provider: provider, uid: uid) do |user|
    # createのときだけ実行される
    user.nickname = nickname
    user.image    = image
    user.name     = Faker::Name.first_name
  end
end
{% endhighlight %}

### その奥にある原因
もっと深掘りしてみると、そもそも、そもそもなんですが、このメソッドの責務が２つあります。
①`auth_hash`を`User`に変換する役割と、②`find_or_create_by`を実行する役割です。
分離しましょう。


{% highlight ruby linenos %}
def self.find_or_create_from_auth_hash(auth_hash)
  user = convert_auth_hash_to_user(auth_hash)
  provider = user[:provider]
  uid      = user[:uid]
  nickname = user[:nickname]
  image    = user[:image]

  User.find_or_create_by!(provider: provider, uid: uid) do |user|
    # createのときだけ実行される
    user.nickname = nickname
    user.image    = image
    user.name     = Faker::Name.first_name
  end
end

def self.convert_auth_hash_to_user(auth_hash)
  User.new(
    provider: auth_hash[:provider],
    uid:      auth_hash[:uid],
    nickname: auth_hash[:info][:nickname],
    image:    auth_hash[:info][:image]
  )
end
{% endhighlight %}

### ここで違和感
この部分。

{% highlight ruby linenos %}
  user = convert_auth_hash_to_user(auth_hash)
  provider = user[:provider]
  uid      = user[:uid]
  nickname = user[:nickname]
  image    = user[:image]
{% endhighlight %}

詰め替えているけれど、そもそも自分自身のインスタンス変数に詰めて、
そして、詰めたインスタンス変数をそのまま使うのが自然じゃないか？
そう考えると、クラスメソッドであるべきじゃなくて、インスタンスメソッドに変えるべきだろう。


{% highlight ruby linenos %}
def find_or_create_from_auth_hash(auth_hash)
  convert_auth_hash_to_user(auth_hash)

  User.find_or_create_by!(provider: provider, uid: uid) do |user|
    # createのときだけ実行される
    user.nickname = nickname
    user.image    = image
    user.name     = Faker::Name.first_name
  end
end

def convert_auth_hash_to_user(auth_hash)
  self.provider = auth_hash[:provider]
  self.uid =      auth_hash[:uid]
  self.nickname = auth_hash[:info][:nickname]
  self.image =    auth_hash[:info][:image]
end
{% endhighlight %}

### 責任を分割
それは、auth_hashで受けないといけないからだ。
その部分と呼び出しを分けよう。

{% highlight ruby linenos %}
def find_or_create_from_auth_hash(auth_hash)
  convert_auth_hash_to_user(auth_hash)
  find_or_create_with_auth_hash
end

def convert_auth_hash_to_user(auth_hash)
  self.provider = auth_hash[:provider]
  self.uid =      auth_hash[:uid]
  self.nickname = auth_hash[:info][:nickname]
  self.image =    auth_hash[:info][:image]
end

def find_or_create_with_auth_hash
  User.find_or_create_by!(provider: provider, uid: uid) do |user|
    # createのときだけ実行される
    user.nickname = nickname
    user.image    = image
    user.name     = Faker::Name.first_name
  end
end
{% endhighlight %}

よくみるとconverという動詞が合わなくなった。
実際には、auth_hashをuserにapplyしている。

{% highlight ruby linenos %}
def find_or_create_from_auth_hash(auth_hash)
  apply_auth_hash(auth_hash)
  find_or_create_with_auth_hash
end

def apply_auth_hash(auth_hash)
  self.provider = auth_hash[:provider]
  self.uid =      auth_hash[:uid]
  self.nickname = auth_hash[:info][:nickname]
  self.image =    auth_hash[:info][:image]
end

def find_or_create_with_auth_hash
  User.find_or_create_by(provider: provider, uid: uid) do |user|
    # createのときだけ実行される
    user.nickname = nickname
    user.image    = image
    user.name     = Faker::Name.first_name
  end
end
{% endhighlight %}




### User.newに知識をまとめるアイディア
User.new(auth_hash)というように、newするタイミングで、入ってくるデータ毎にどう扱うべきか？の知識をUserが持っていることもできる。むしろ、new/initializeにもたせておけば、呼び出し側はいつも意識せずに呼び出せて、知識が疎結合になってよい。けれど、実際に、そういうコードを書くことはActiveRecordでは想定されていないみたいで、`find_or_create_by(attributes, &block)`でブロックが計算されなくなってしまった。

{% highlight ruby linenos %}
alias :old_initialize :initialize
def initialize(attributes = nil)
if attributes && attributes[:info] then
  attributes[:nickname] = attributes[:info][:nickname]
  attributes[:image]    = attributes[:info][:image]
  attributes.delete(:info)
end
old_initialize(attributes)
end
{% endhighlight %}
