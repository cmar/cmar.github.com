---
layout: post
title: "Debugging Spree 1.0 with Ruby 1.9.3"
date: 2012-02-28 07:17
comments: true
categories: [Spree]
---

Here are the steps you need to get ruby-debug to work with Ruby 1.9.3p125 in the Spree sandbox.

Setup
-----
* rvm 1.10.3
* ruby 1.9.3p125
* Spree 1.0.x

Installing Gems
---------------
``` ruby
# download the source code for pre-release version of linecache, then install locally
curl -OL http://rubyforge.org/frs/download.php/75414/linecache19-0.5.13.gem
gem install linecache19-0.5.13.gem

# download the source code for pre-release version of debug-base, then install locally
# using rvm installed source code (/Users/cmar/.rvm/rubies/ruby-1.9.3-p125/rubies/src)
curl -OL http://rubyforge.org/frs/download.php/75415/ruby-debug-base19-0.11.26.gem
gem install ruby-debug-base19-0.11.26.gem -- --with-ruby-include="${MY_RUBY_HOME/rubies/src}"
```

Modify Spree Gemfile
--------------------
``` ruby
group :development do
   gem 'linecache19', '0.5.13'
   gem 'ruby-debug-base19', '0.11.26'
   gem 'ruby-debug19', :require => 'ruby-debug'
end
```

Running Spree Sandbox
---------------------
After you update the Spree gemfile, you can create a sandbox and run with the debugger
``` ruby
git clone https://github.com/spree/spree
cd spree
# edit Gemfiile (above)
bundle
bundle exec rake sandbox
cd sandbox
rails s --debugger
```

