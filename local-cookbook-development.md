You shouldn't do this kata until you've mastered the [Basic Chef Fluency Kata](basic-chef-fluency.md).

# References

* [Matt Stratton's Workflow using Local Delivery](https://www.mattstratton.com/post/getting-started-with-chef/)

# Kata

## Create Private Chef Infrastructure

* Create a new [chef_repo](https://docs.chef.io/chef_repo.html) with a connection to a new [manage.chef.io](http://manage.chef.io) [Organization](https://docs.chef.io/server_orgs.html)
* Create a Chef Server in Azure using a wrapper cookbook to the [chef-server cookbook](https://github.com/chef-cookbooks/chef-server)
  * Use [kitchen-dokken](https://github.com/someara/kitchen-dokken) as a driver
  * This should be built using an ARM template to create and bootstrap, using a [manage.chef.io](https://manage.chef.io) account. Once you create the ARM template the first time, clone it in all future kata runs.
* Create a Private Chef Supermarket in Azure using a wrapper cookbook to the [supermarket-omnibus-cookbook](https://github.com/chef-cookbooks/supermarket-omnibus-cookbook). See also [Irving's post](http://irvingpop.github.io/blog/2015/04/07/setting-up-your-private-supermarket-server/) for help.
  * Continue to use `kitchen-dokken` when testing the cookbook
  * Create another ARM template for creating and bootstrapping this in Azure, using your [manage.chef.io](https://manage.chef.io) account.
* In the private Chef Server, log in as the administrator and add a new Organization
* Log in as a user and request an invitation to an Organization
* Log in as the administrator and grant the invitation
* Log in as a user and access the organization. Set up a new connection to this chef server

## Create Cookbook

### Workflow Description

For all cookbooks below, follow these rules:

* Run `foodcritic` before every checkin
* Run `rubocop` before every checkin (and autocorrect with it when you have issues; also know the difference between `rubocop` and `cookstyle`)
* Run Delivery Local Lint to automate `foodcritic` and `rubocop` runs
* Ensure that kitchen runs (with Delivery Local as well)
* Use `dokken` runner to speed up your kitchen runs

### Steps

* Ensure that all recipes created have `# Authored by: [Your Name]` at the top of the recipe (using a `chef generate` template)
* Create a `platform` cookbook on version `0.1.0`
* Add a `platform` custom resource that installs `nano` and `curl` packages called `base_utilities`
* Test the `platform` custom resource with [an embedded test-only cookbook and recipe](http://stackoverflow.com/questions/26153197/testing-library-chef-cookbooks)
* Upload `platform` to your private supermarket
* Create a `website` cookbook on version `0.1.0`
* Write a ChefSpec test that ensures `nano` and `curl` packages are installed by your `default` recipe
* In the `default` recipe, ensure that the base utilities are installed
* Write an inspec test, test it with test kitchen
* Using your private supermarket, upload `website` and `platform` to the Chef Server, converge a node with `knife bootstrap` and run your inspec profile to test that it converged properly
* Freeze the version of the cookbook so it doesn't accidentally get written over
* In the platform cookbook add an action to `base_utilities` that would remove those packages
* Use `knife spork` to increment the version to `0.1.1`
* Upload the new version of the `platform` to the supermarket
* Add a `rollback` recipe to `website` that would remove it
* Test the `rollback` recipe in another kitchen suite
* Increment the `website` version to `0.2.0` (with `knife spork`)
* Upload to the chef server with frozen version, reconverge machine and see that nothing changed
* Roll back the website by changing the run list of your node to the new recipe, converging again, and see the packages removed
* Include the `chef-client` recipe in your `default` recipe and make chef run every 30 minutes. Override the chef-client attributes in the recipe itself.
* Have berks get the `chef-client` recipe from GitHub (it won't be on your private supermarket)
* Ignore `foodcritic` rule `FC002` in your entire `platform` cookbook
* Ignore `foodcritic` rule `FC002` in just your `rollback` recipe
* Change the max line length for rubocop to `500` for your entire `platform` cookbook
* Change kitchen to use `vagrant` and `virtualbox` for your `website` cookbook and ensure that `centos` and `ubuntu` are tested platforms
* Your `default` recipe should install `chocolatey` and then `notepad++` chocolatey package if it is a windows node (update the underlying resource). Test this on Windows 2016.
* Update your ChefSpec test to work with windows as well
* Write a file out to your file cache directory that has a completely different format if `centos`, `ubuntu`, or `windows` but writes the same variable data out from an attribute
* Create another recipe `security` that will write out a security-related attribute to that same file (see [partial template](https://docs.chef.io/resource_template.html#partial-templates))
* Write out a list of names of five people you admire to this file as well
* Write a file out to the file cache directory named `memory.txt`. If the memory on the node is greater than 8GB, write out "this machine has lots of memory", otherwise write out "this machine doesn't have a lot of memory"
* Save the computer's password to the Chef Server using [chef-vault](https://docs.chef.io/chef_vault.html). You should still be able to test this with kitchen using the data bag fallback.
* View this password with `knife vault show` command
* Write this password out in clear text to `password.txt` in your file cache directory
* Write all nodes that are running `ubuntu` to a file `ubuntu_nodes.txt` in the file cache directory (using `search`). Make sure this doesn't break your kitchen runs
* On ubuntu nodes only, run `lsb_release -r` and write the results to a file, without using the execute resource. See https://docs.chef.io/ruby.html#shelling-out

## Kitchen

* Create a kitchen run with `shell` provisioner and `busser` test that will install `nano` and test that it's installed (using [kitchen init](https://docs.chef.io/ctl_kitchen.html#kitchen-init))

