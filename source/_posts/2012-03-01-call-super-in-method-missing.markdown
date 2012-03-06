---
layout: post
title: "Call super in method_missing"
date: 2012-03-01 14:07
comments: true
categories: anti-pattern, ruby
---

Recently, I was refactoring some code and came across this seemingly innocuous method_missing override:

```ruby
def method_missing(name)
  @properties[name]
end
```

I've seen this **quick** hack more often then I'd prefer. It allows easy reader access to the values stored in the hash.

```ruby
#instead of this
object.properties[:keywords]

#you can call this
object.keywords
```

The *keywords* method is not found; so, method_missing is called. The missing method name is passed to the hash and the value is returned. But if the key does not exsist, then nil is returned. The side effect is that any method name can be called on the object and nil is returned. Even inncorect method names will happily return nil. This can propogate misspelled method names throughout the code.

Overriding method_missing is a great way to quickly add convenient reader methods to your object. But method_missing has an important job. By not calling super, your changing the assumed behavior native to ruby. Misspelled method names or methods which do not exist should raise an exception. This is the expected behavior.

The correct way to override method_missing is to check if the key exists; otherwise call super.

```ruby
def method_missing(name)
	if @properties.has_key? name
  		@properties[name]
	else
		super
	end
end
```


