# References

* https://docs.chef.io/ruby.html

# Ruby Basics

* **Primitives:** built in data types: `string`, `integer`, `FixNum`, `boolean`, and `symbol`
* **Variables:** storage container for data
* **Collections:** two types: `array` (lists) and `Hash` (key/value pairs). Can hold any data
* **Methods:** resuable behavior. Data can be passed in via _parameters_. A method can have a return value.
* **Class:** reusable behavior and data

# Kata

* Using the `puts` method, greet yourself by name
* Put your name in a variable (and keep the greeting)
* Put your employee number in the greeting as well
* Create a list of your hobbies
* If you have more than 5 hobbies, the program should say that you have too much going on and that you need to get rid of something
* If you have no hobbies, the program should say that you need to learn something
* Create a method `comment_on_hobbies` that will contain the previous two steps
* Create a method `greet` that greets you by name
* If the name has `Ann` inside of it, the greeting should say "your name is a lot like the creator of this program!" (use a regular expression)
* Create a `Person` class that contains a `name`, `employee_number`, `hobbies`, and the `comment_on_hobbies` and `greet` methods
* Make sure your code is properly commented
* Create the people who work in your office by name (as a `Hash`)
* Have the user enter in a name, and greet them and comment on their hobbies
* In addition to the current greeting, when a hobby is 'cooking', comment "I like cooking too!". When the hobby is "mountain biking", "biking", or "rock climbing", say "I'm not as into those things"
* Write a method `original_employee?` where it returns `true` if your `employee_number` is under `20`.
