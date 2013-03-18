---
layout: post
title: "bundle searching"
date: 2013-03-18 10:27
comments: true
categories: codlin
---

Jim Gay ([@saturnflyer](https://twitter.com/saturnflyer)) posted a [great tip](http://www.saturnflyer.com/blog/jim/2013/03/15/searching-through-your-bundled-gems) for using [The Silver Searcher](https://github.com/ggreer/the_silver_searcher) to find the occurrence of a word within the gem dependencies for your project. 

Bundler has had requests for search capabilities. Rule #2 of the Unix Philosophy is make each program do one thing well. The Silver Searcher is my current favorite searching tool. Its replaced grep and ack for all my code searching. Following rule #2, we don't need bundler to search for us, we just need to compose it together with the best search tool.

Its a simple idea. Jim Gay wrote about it. I thought it was brilliant. I created an alias to make it easy to use:

```
alias bs="bundle show --paths | xargs ag"
```

Now, inside a project I can run 'bs user' to search for the word 'user' within all the gems included in my project. Is this fast? Yes! Inside a Spree sandbox - which has 81 gem dependencies - I can search for a common word like 'user' in 3.339s. 

Thanks for the idea Jim! 


