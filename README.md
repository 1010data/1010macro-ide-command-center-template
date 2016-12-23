# 1010macro-external-dev-template
Boilerplate template to provide grunt tasks to:
* run a local server to test quickapps in an iframe. When files change upload to 1010 and then refresh browser via [LiveReload](http://livereload.com/).
* deploy application into 1010 (includes creating folders if they do not exist)
* run queries and see the results in console (via tendo)


##Requirements:
* [Nodejs](http://nodejs.org/)
* [Grunt](http://gruntjs.com/)
* [Tendo](http://www.1010data.com/)

##Optional
* [LiveReload](http://livereload.com/) to automatically refresh browser when files change. 

## Getting Started
Install the [Nodejs](http://nodejs.org/), [Grunt](http://gruntjs.com/) and [Tendo](http://www.1010data.com/)  


Install the [Nodejs](http://nodejs.org/) modules used by the project (from the project root folder):
```js
    npm install
```    


If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins.

Install the grunt command line interface (globally):
```js
    npm install -g grunt-cli
```    

## Quick usage examples:
Before diving into any of the configs lets see some quick examples!:
 
1. First deploy the app:
```js
    grunt deploy
```    

2. Now start the local server, watchers, livereload, etc:
```js
    grunt serve    
```
    
3. Open a browser and navigate to (http://localhost:8000/hello.world.html). The "Hello World" quickapp will be loaded into the html iframe.

4. Now open 'src/app/hello.world.xml' and change 'Hello World' to 'World Hello' and save.  The serve command will detect the file has change, rebuild it, push it up to 1010 and then refresh the browser ([LiveReload](http://livereload.com/) extension required).
**NOTE** We are still awaiting a fix to allow the iframe to login by possessing the session without needing user interaction.  Currently it will log you in but come back and ask how you want to create the session.   
   
   
##Notes on TenDo
* TenDo tasks are executed via the [grunt-tendo](https://www.npmjs.com/package/grunt-tendo) npm package (which internally uses the [tendo](https://www.npmjs.com/package/tendo) npm package).   The grunt-tendo task configurations are located in 'tasks\configure.tendo.js' and additional configuration values can be found 'build.config.js'. 

## Auto-managed quick queries:
Inside of the 'build.config.js' there is a node named 'quick_queries' this is where the auto-managed quick queries are defined.  Each node will automatically have a 'watch', 'tendo' and optionally a 'replace' task created.  
Example:
```js
 quick_queries: {
    ...
        lib1: {
            table: '<%=root_path%>.lib1.lib1\(Library 1\;\;\)=<%= basetable %>',
            src: ['<%= app_dir %>/lib1/lib1.xml']
        }
     ...
}
```
This is the minimal configuration needed to manage the lib1.xml quick_query.  
To manually deploy the query:
```
    grunt tendo:lib1
```


## Grunt tasks:

 #### Deploy task
 Deploys the application into 1010 creating any folders needed.  The root of the application is declared as 'root_path' in 'build.config.js'.
```
    root_path: 'pub.consumer_data.oi.internal.workspace.<%= login.id %>.app',
``` 
 login.id will be replaced with the user id used to run the grunt command.
 
 
 To run:
 ```
     grunt deploy
 ```    

#### Serve task
Can be used to automate file changes (deploying, tokenizing, etc). 

* Starts a 'connect' http webserver locally ([http://localhost:8000/](http://localhost:8000/)) (see 'tasks/configure.connect.js') ,
* Watches for file changes (see 'tasks/configure.watch.js) and runs configured tasks.
* Notifies 'LiveReload' browser plugin to refresh page (see 'tasks/configure.connect.js').  Also, see [livereload.com](http://livereload.com) for more info.

To run:
```
    grunt serve
```    

#### Weather task
This task is an example of running a query and logging the results to the console. It also shows how to manually create your own 'watch' and 'tendo' tasks. See the 'weather' node in 'tasks/configure.tendo.js'. 

To run:
```
    grunt weather
```