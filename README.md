# Introduction

Learning Chef can be difficult with real world examples, because infrastructure automation has so many complicated dependencies to them. Instead, I think it's better when learning Chef to learn by doing simple tasks.

People learn better by doing simple examples. Let's help them get there when learning Chef.

# Badges

I'm not a huge believer in certifications, but I think the Chef certification is a fantastic way to ensure that you cover all of the elements of Chef. As a result, I'm using the Chef badges as a guide for following through the examples.

For more information on the certification, [watch this video](https://www.youtube.com/watch?v=Snjb_eUxsgA).

# Basic Chef Fluency Badge

## Resources, Recipes, Cookbooks
* Create a `chef-training` repo on GitHub and clone it locally
* Ensure that you have a code editor with Chef Plugins installed. I recommend Visual Studio Code.
* Generate a cookbook into the `chef-training` repo
* Make your cookbook only support ubuntu
* Set up test kitchen to run the `default` recipe of your cookbook using vagrant and virtualbox
* Ensure that nano is installed (in an inspec test and recipe). Run kitchen converge and verify to ensure this works.
* For the rest of the lab, create a Test Kitchen workflow that uses the `kitchen create`, `kitchen converge`, `kitchen verify` and `kitchen destroy` commands. Also use `kitchen login` to manually ssh into your ubuntu machine.
* Create `/var/website` directory
* Make sure `/var/old-website` directory does not exist
* Write a file `/var/website/directions.txt` with text "website goes here" in it
* Write a file `builder.txt` to `/var/website/builder.txt` containing the text "[Your Name] built this" where `[Your Name]` is a cookbook attribute with your actual name.
* Download the Chef logo into `/var/website`: https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSgQmQ0CYwU3cpFE6gEB82cp6TSIcBJSisax_HVvEfsgYHGBsO8kQ
* When you run test kitchen, `builder.txt` should contain the text `Test Kitchen built this`
* Run the command `echo ran command > /var/website/command.txt`
* Don't run the command the second time chef converges (i.e. make it idempotent)
* If the command *does* run, do a git pull of the architect repository into `/var/website/architect` (https://github.com/pages-themes/architect). It shouldn't pull the repository every time.
* Refactor your command and pull into a custom resource called `chef_training_website`
* Make the git repo that you pull an attribute
* Write a `MyLogger` class with a `Log` method that prepends the message `CHEF TRAINING: ` and outputs that to the STDOUT (using puts).

## Chef Server

Now that we've had some practice with basic cookbook development, let's get connected to a Chef Server

* Create an ubuntu virtual machine with VirtualBox
* Set up an account on manage.chef.io and ensure your keys and knife.rb are available to your `knife` command on your workstation
* Ensure your `chef-training` cookbook is uploaded to the Chef Server
* Bootstrap your machine running 2 recipes: `chef-training` and the `os-hardening` cookbook. You'll need to ensure the other cookbook is uploaded to the Chef Server as well.
* Run `chef-client` on the machine again, noticing that 0 resources are converged the second time

## Search
* On your workstation, search for all ubuntu nodes
* On your workstation, search for all nodes that match the attribute used to create the `builder.txt` file above
* Create a data bag `website` with item `messages`. Inside of `messages`, have a `welcomeMessage` named `Welcome to Chef Learning!`
* In your `chef-training` cookbook, write a file `/var/website/welcome.txt` with the welcome message from the data bag.
* Push the updated cookbook to the Chef Server and reconverge, ensuring that the file is there on your ubuntu VM.
* Update the data bag to `Welcome to the BEST Chef Learning EVER!`
* Reconverge and see that the file changed

## Advanced Administration

* Show the node's run list with `knife` and look it up in the UI
* Create a role named `security` which includes the `os-hardening` cookbook
* Read over the README of the `os-hardening` cookbook and find some attributes to set. Set those attributes in your `security` role.
* Change the run list on the command line to remove the `os-hardening` cookbook and add the `security` role
* Reconverge and ensure that the behavior is the same
* Create a `development` environment that will be assigned to your existing node. It should:
  - Always run the latest `chef-training` cookbook on the chef server
  - Run the `1.4.1` version of `os-hardening` cookbook
  - The `builder.txt` should say `Development Built This`
  - Assign the `development` environment through the `client.rb` on your virtual machine
* Create another virtual machine that will be your "production" machine
  - Run a specific version of the `chef-training` cookbook
  - The `builder.txt` should say `Production Built This`
  - Assign the `production` environment through the `client.rb`
  - Update your `chef-training` cookbook to change the text of `builder.txt`. After uploading it to the Chef Server notice that only your `development` node was updated, but not your `production` node.

## Chef Automate

For the first badge, you need to understand Automate itself, so this section won't be example driven, but more idea driven.

Watch [this video](https://www.youtube.com/watch?v=ldY7KEOxCkM&index=1&list=PL11cZfNdwNyOPa_kLgCX0wDW3O00Sjydx) to get an overview of Chef Automate.

Now do this thought experiment:
* How would you solidify your cookbook deployment workflow? (if you don't know workflow enough, watch [this video](https://www.youtube.com/watch?v=OdoGu31EBU0))
* How would you see what happens on your nodes?
* How would you scan your nodes with inspec profiles?
