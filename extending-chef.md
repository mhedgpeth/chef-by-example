
# Ohai

* Write a custom Ohai plugin (that has the docker plugin in it)
* Test ohai with `chef-shell` or `irb` (know which one)
* Deploy it with the `ohai` cookbook using the `ohai_plugin` resource
* Reload the plugin once a package is installed
* depend on kernel data
* Put docker processes in ohai as a nested hash
* Add some azure data as a hint and use the hint to add data to a plugin
* Disable the plugin
* Remove some ohai content
* Whitelist an attribute

# Custom Resources

* `use_inline_resources`

# Chef Handlers

* Write the result of a chef run to a file called `results.txt` locally
* Deploy the handler with the `chef_handler_cookbok` and `chef_handler` resource

# Libraries

* Write a library
* Write a resource and provider with a library
* Extend chef with a library

# Definitions

* Write a definition (and know why you shouldn't)

# Knife Plugins

* Inherit from a knife plugin

# Chef API

* Talk to the Chef Server via an API

# Ruby

* Create a ruby gem
* Manage ruby gems