---
layout: post
title: "Serving Additional Assets on the Pipeline"
date: 2012-05-17 09:12
comments: true
categories: rails, asset pipeline
---
# What is the problem?
Javascript and Stylesheet files required in application.js/css are not directly
accessable in production. This is hard to detect during development because
all assets are accessable through /assets/{name} in development mode.

The Rails Golden Path expects all javascript and stylesheet files to be
declared in application.js/css. They are compiled together and minified
in production. Once they are compiled together in production, the individual
files are not directly accessable through /assets/{name}.

There are a couple situations where this causes problems. If you include a
3rd party javascript library that includes additional files, they may be
accessable in development, but quietly fail in production.

We were using redactor.js which has 4 support files. It uses javascript
to include the supporting files. We updated the declarations to /assets/en.js,
/assets/pages.js etc. This worked fine in development. Redactor could access
/assets/pages.js but in production when it was trying to request
/assets/pages.js was returning 404.

We required pages.js at the top of application.js. It was getting compiled
together with the rest of the javascripts, but Redactor quietly failed when
it couldn't access /assets/pages.js.

# Why did this happen?
Rails assumes you will declare all your javascript and stylesheets in application.js/css.
It also assumes in development that you want to access all the files directly through
/assets/{name}. But once all the files are compiled to the /public/assets/application.js,
you don't need direct access to the individal files. The direct access we enjoyed in development
is no longer available.

In production, some configuration options are set to improve performance:

	# Disable Rails's static asset server (Apache or nginx will already do this)
	config.serve_static_assets = true

	# Compress JavaScripts and CSS
	config.assets.compress = true

	# Don't fallback to assets pipeline if a precompiled asset is missed
	config.assets.compile = false

The last line says that if a file isn't found in /public/assets don't look in the
project asset directories to find it. This is true in development.
Any file burried in an assets directory can be found.

When you deploy to production, 'rake assets:precompile' is run. All the files declared
in application.js will be compiled and minified into a single file in /public/assets. But
the individual files will not be copied into /public/assets.

# How do you fix it?
While you are in development mode, you can run 'rake assets:precompile'. Then inspect
your public/assets directory to see which files are copied over. If a specific file is
not there; it will not be available in production.

If you need a specific file compiled and accessable from public/assets you need to
declare it in 'config/enviroments/production.rb':

	# Precompile additional assets (application.js, application.css, and all non-JS/CSS are already added)
	config.assets.precompile += %w( myfile.js )

If you need a javascript or stylesheet file to be directly accessable from public/assets, then you must
add it to the array passed to __config.assets.precompile__. Once you add it, you can run
'rake assets:precompile' again to verify the file is copied over.

The trick is that the files are directly accessable in development mode so config.assets.precompile
is not used. But without explictly declaring them in production.rb, they will not be available once you've
deployed to production.

Stay on the Golden Path.

