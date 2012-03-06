---
layout: post
title: "Spree Product Groups Removed"
date: 2012-03-06 09:56
comments: true
categories: spree
---

We have removed Product Groups from the core Spree project. This was a rarely used feature; but, the code was fairly complex. In an effort to keep Spree lean, the code was extracted and moved to an [extension](https://github.com/spree/spree_product_groups).

The commit to [Spree Master](https://github.com/spree/spree/commit/b0e1b664f4dc87efeb54333e4cfe7802d0d9ebfc) will be part of the 1.1 release. If you are using Product Group functionality, then you'll want to include the extension in your Gemfile after 'spree'.

```ruby
gem 'spree'
gem 'spree_product_groups', :git => 'git@github.com:spree/spree_product_groups.git'
```

It has not been released as a gem, so you'll have to declare it with a reference to the git repository.

All previous Product Group functionality will be restored by the extension.