---
layout: post
title: "Enabling YJIT"
date: 2024-04-23
---

## Introduction

I just did this today. So, this post will not include the results of enabling YJIT.

As finance folks say

> This is not ~~investment~~ programming advice.

## Why

In one acronym, **FOMO**.

A while back I read this PR, [Enable YJIT by default if running Ruby 3.3+](https://github.com/rails/rails/pull/49947){:target="_blank"}. It made it look like a low hanging fruit. I thought, why not?

The why yes list included:

- [This Shopify post](https://shopify.engineering/yjit-faster-rubying){:target="_blank"}
- [This Rails at Scale (also Shopify) post](https://railsatscale.com/2023-12-04-ruby-3-3-s-yjit-faster-while-using-less-memory/){:target="_blank"}
- [This 37 signals post](https://dev.37signals.com/yjit-is-fast/){:target="_blank"}

## How

### On the server(s)

My Ruby on Rails (Ruby 3.3.0 -via `rbenv`-) app was running on an Ubuntu server.
So, what I did was:

First, I checked if my Ruby had YJIT. I did not remember if I had installed it with it 🤷‍♂️.

```shell
ruby --enable-yjit -v
```

I didn't. As a matter of fact, I did not have Rust installed either. So, I did that first.

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

And then I added Rust to my path.

```shell
nano ~/.bashrc
```

```text
export PATH="$HOME/.cargo/bin:$PATH"
```

```shell
source ~/.bashrc
```

```shell
rustc --version
```

Finally, I installed Ruby with YJIT.

```shell
RUBY_CONFIGURE_OPTS="--enable-yjit" rbenv install 3.3.0
```

### On the app

I just added the initializer to enable YJIT (taken from the PR mentioned above ☝️).

```ruby
# config/initializers/enable_yjit.rb

if defined? RubyVM::YJIT.enable
  Rails.application.config.after_initialize do
    RubyVM::YJIT.enable
  end
end
```

## Conclusion

Everything went so smooth that it feels like the fruit fell into my hands.

I did not mentioned it because it's kind of a given, but having a good test coverage provides a lot of confidence when trying out this things. And, of course, I tried everything on my local machine first and on my staging server later.

The only regret so far is not having done this 30 minutes later. I finished the whole thing just minutes before [Ruby 3.3.1 was released](https://github.com/ruby/ruby/releases/tag/v3_3_1){:target="_blank"} 🤦‍♂️.
