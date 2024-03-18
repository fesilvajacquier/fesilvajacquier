---
layout: post
title: "Script Belt: Detecting Funky Comparisons"
date: 2024-03-18
---

## Introduction

Welcome to another installment of "Script Belt," where I share useful scripts that I regularly use for development or debugging. Today, we're diving into a Ruby script that helps you detect funky comparisons in a Rails application. Probably this is very specific to my use case, but I have shared with some people that found some bits useful.

I wrote this script with the help of my friends at [RubySur](https://rubysur.wenoa.studio/){:target="_blank"}. I posted the first version of this script in our Slack channel and they helped me improve it.

## Why Use This Script?

This one was born to help me check a reported bug was not happening in other places of the code.
At some point we were comparing `Date` with `Time` and that was causing some issues. This script is helpful to find all the places where that was happening.

## The Script

Here's a part of the script that I included in my `test_helper.rb`. With this bit you might get the gist of it.

```ruby
# test_helper.rb

# ...
# Custom logger for comparison operations
COMPARISON_LOGGER = Logger.new(STDOUT)
COMPARISON_LOGGER.level = Logger::WARN

class Date
  [:<, :<=, :==, :>, :>=, :===].each do |operator|
    define_method(operator) do |other|
      if other.class.in?([ActiveSupport::TimeWithZone, DateTime, Time])
        COMPARISON_LOGGER.warn "Comparison of Date with #{other.class} detected"
        caller_locations.each do |location|
          # Filter out traces from dependencies (gems)
          if location.absolute_path&.include?(Rails.root.to_s)
            COMPARISON_LOGGER.warn "#{location.absolute_path}:#{location.lineno}"
          end
        end
      end
      super(other)
    end
  end

  # patch methods that might not take just `other` as an arg

  [:between?, :clamp?].each do |operator|
    define_method(operator) do |*args|
      if other = args.detect { |arg| arg.class.in?([ActiveSupport::TimeWithZone, Range, DateTime, Time]) }

        COMPARISON_LOGGER.warn "Comparison of DateTime with #{other.class} detected"
        caller_locations.each do |location|
          # Filter out traces from dependencies (gems)
          if location.absolute_path&.include?(Rails.root.to_s)
            COMPARISON_LOGGER.warn "#{location.absolute_path}:#{location.lineno}"
          end
        end
      end
      super(*args)
    end
  end
end
```

## How It Works

1.  **Custom Logger**: The script uses a custom logger to print warnings when a comparison operation is detected between `Date` and `Time` classes.
2.  **Monkey Patching**: It then monkey patches the comparison methods of the `Date` class to log a warning when a comparison operation is detected between `Date` and other classes.
3.  **Printing the Call Stack**: If such a comparison operation is detected, the script prints the call stack to the console, filtering out traces from dependencies to make it easier to identify where the comparison is being made.

## How to Use It

Simply add this script to your `test_helper.rb`, run your tests (`rails test:all`) and check if anything is logged to the console. If you see any warnings, you can use the printed call stack to identify where the comparison is being made.
