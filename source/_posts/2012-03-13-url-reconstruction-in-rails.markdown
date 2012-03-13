---
layout: post
title: "url reconstruction in rails"
date: 2012-03-13 10:50
comments: true
categories: rails
---

## Problem
In Rails, I needed to take the request url and redirect the user to a new host with additional query parameters while passing the original path and query parameters.

For example, if the original request looked like:

	"http://www.mystore.com/cart?sort=desc"

I needed to redirect them to:

	"https://mobile.mystore.com/cart?sort=desc&view=ipad"

While this could be done with fancy string manipulation, I figured there had to be a cleaner and less error prone way.

## Solution
There is a great utility in [Ruby Core](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/uri/rdoc/Kernel.html#method-c-URI) to handle URL manipulation. Unfortunately, it isn't well documented.

There is a kernal level method named URI which parses the full url and breaks it down into parts

	# request.url = http://www.mystore.com/cart?sort=desc
	uri = URI(request.url)
	puts uri.host # www.mystore.com
	puts uri.scheme # http
	puts uri.query # sort=desc

To swap the host and scheme was easy enough

	uri.host = "mobile.mystore.com"
	uri.scheme = "https"

To modify the query parameters, we had to consider the possiblity of no parameters. In that case, the uri.query would be nil. The query parameters are only broken down into a string, so we have to split, add and join them back together.

	query = uri.query ? uri.query.split('&') : []
	query << "view=ipad"
	uri.query = query.join('&')

Then we just have to reconstruct the url and redirect the user

	redirect_to uri.to_s # https://mobile.mystore.com/cart?sort=desc&view=ipad

## Conclusion
There is always a cleaner way.



