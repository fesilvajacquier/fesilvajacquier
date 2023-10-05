---
layout: post
title: "Script Belt: Detecting Model Creation in Rails"
date: 2023-10-05
---

## Introduction

Welcome to another installment of "Script Belt," where I share useful scripts that I regularly use for development or debugging. Today, we're diving into a Ruby script that helps you detect when a new instance of a model is created in a Rails application. This can be incredibly useful when you're about to make significant changes to the data layer of a crucial model.

## Why Use This Script?

Before I dive into any major changes to a model, I like to have a clear understanding of where and how new instances of that model are being created. This is especially important for me when:

- I want to make sure that my changes won't disrupt any critical processes, like payment creation.
- I'm adding new attributes to the model and want to ensure they are set at the time of creation.
- And, specially, when there are many points of entry to create a new instance of the model. For example: when notifications from external services result in the creation of a new instance of the model.

## The Script

Here's the Ruby script that I include in my `test_helper.rb`:

```ruby
def detect_model_creation(model_class)
  # Subscribe to the sql.active_record event
  ActiveSupport::Notifications.subscribe("sql.active_record") do |*args|
    event = ActiveSupport::Notifications::Event.new(*args)
    # Parse the SQL query to detect when a new instance of the model is created
    if event.payload[:sql].include?("INSERT INTO \"#{model_class.table_name}\"")
      # Print the call stack to the console
      puts "--------------------------------------------"
      puts "New instance of #{model_class.name} created!"
      caller_locations.each do |location|
        # Filter out traces from dependencies (gems)
        if location.absolute_path&.include?(Rails.root.to_s)
          puts "#{location.absolute_path}:#{location.lineno}"
        end
      end
      puts "--------------------------------------------"
    end
  end
end

# Call the method with the Payment model class
detect_model_creation(Payment)
```

## How It Works

1.  **Subscribing to Events**: The script uses `ActiveSupport::Notifications` to subscribe to the `sql.active_record` event. This event is triggered whenever an SQL query is executed.

2.  **Parsing SQL Queries**: It then checks if the SQL query includes an `INSERT INTO` statement for the table corresponding to the model class you're interested in.

3.  **Printing the Call Stack**: If such an SQL query is detected, the script prints the call stack to the console, filtering out traces from dependencies to make it easier to identify where the new instance is being created.

## How to Use It

I simply add this function to your `test_helper.rb` and call `detect_model_creation` with the model class I'm interested in. For example, to track the creation of `Payment` instances, I call `detect_model_creation(Payment)`.

## Conclusion

This script has been a handy tool for me to understand where new instances of a model are being created in my Rails applications. It's particularly useful when I'm about to make changes to a model and want to ensure that I'm not breaking anything critical.

Just a heads-up: this script assumes you have good test coverage, as it relies on running tests to detect model creation events. It works well for me, and I hope you find it interesting!
