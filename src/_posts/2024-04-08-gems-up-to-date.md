---
layout: post
title: "Keeping gems up to date"
date: 2024-04-08
---

## Introduction

Nothing to fancy here. Just the process I use to keep my gems up to date.

## When

I have a recurring task in my calendar that reminds me to update the gems in my projects.
It is set to repeat itself every 3 months.

## Why

Of course there are many reasons like **security**, **performance**, **bug fixes**, etc. But the main reasons for me are:

1. **Pay the technical debt** in comfortable installments. This way when the next **Ruby** or **Rails** version is released I will have less work to do.
2. **Check for dependencies to remove**. Dependency minimalism brings me peace of mind.
3. **Discover new functionalities**. Sometimes I find out that a gem I use has a new feature that I was not aware of.

## How

First I run `bundle outdated --only-explicit` to see which gems have new versions available.

```
Gem             Current  Latest  Requested           Groups
faker           3.3.0    3.3.1   ~> 3.3              default
pagy            7.0.11   8.0.2   ~> 7.0, >= 7.0.11   default
rake            13.1.0   13.2.1  ~> 13.0, >= 13.0.6  default
rubocop         1.62.1   1.63.0  ~> 1.62, >= 1.62.1  development
```

Then, for each gem I:

1. Look for it in the [RubyGems](https://rubygems.org/){:target="_blank"} website.
2. Check the CHANGELOG.
3. Update the gem in the `Gemfile`.
4. Run `bundle update gem_name`.
5. Run the tests.

Usually it is a smooth process. But sometimes I have to fix some deprecation warnings or even some tests.

## Conclusion

I told you, nothing fancy. It is a simple process that I do not mind doing. It is a way to keep my projects up to date and to learn new things.
