# All-Inclusiv
Express Middleware for Mobile Apps to be used by very lazy people


#Usage

Install module

	npm install express-all-inclusive --save

In app:

	var static = require('static');

## Folders

Your mobile app might have multiple endpoints:
project, each inside a single folder, e.g.

- www
- landing
- adm

Registering a folder to serve its content:


	static.addFolder('www', { //options object
	  version: 'v1.0.0'
	});

The folder is now available via `static.www`

### Options ###

**version**: will replace `<!--version-->` in appcache and html files, e.g.:

	CACHE MANIFEST
	# Generated: <!--version--> 

becomes

	CACHE MANIFEST
	# Generated: v1.0.0 

To deploy a new app version, increase the app version.

## Files
So far there is build in support for `html`, `js`, `css` and `appcache` files.

### Simple serve ###

	app.get('/app.js', static.static.serve());

This will server the `/static/app.js` file.

### Advanced serve ###
If you want to server files only in special cases, pass req and res parameters on the the serve method.

	app.get('/cache.appcache', function(req, res){
	  if (false){
	    res.send('');
	  } else {
	    static.static.serve(req, res, {});
	  }
	});


### Options ###

**fileName**

By default

	app.get('/test', static.static.serve());

would serve the text.html file. You can change this behaviour by either passing the fileName directly or as object property.

	app.get('/test', static.static.serve('testPage.html'));
	//equals
	app.get('/test', static.static.serve({fileName:'testPage.html'}))

**include**

For mobile applications it is nice to save some requests, there for you can merge files.

    static.static.serve(req, res, {
      include: ['app.js', 'style.css']
    });

This code would replace in production inside index.html

`<script src="app.js"></script>` with `<script>//Minified JS code</script>`

and

`<link href="style.css" rel="stylesheet">` with `<style>/* Minified CSS code*/</style>`

### Bonus features ###
Files are served differently on localhost and in production. In production

-  js files are ng-annotated and uglyfied
-  css files are minified
-  html files are minified
-  js and css files and be merged into html files

On localhost

- appcache files are not served

You can emulate production on localhost by added the `?force=true` parameter to all requests.


## Auto-Reload
Since we are so lazy, auto-reload is build in for html files.

To enable auto-reload add as last statement in your Express app:

	if (localhost){
	  var port = app.get('port') + 1;
	  static.watch();
	}

Add to every html file to be watched:
	
	<!--reload-->

By default, all files inside the registered folders are watched. You can register files via `static.addFolder`. 
You can specify the watched folders by passing an array of folders.

	static.watch(['adm', 'static']);
