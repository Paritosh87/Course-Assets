Exercise-1
---
* If you haven’t already, install both `grunt` and `gulp` (both globally and locally) and create your initial Gruntfile.js and gulpfile.js

#### Solution:
- npm install --save-dev grunt 
- npm install -g grunt-cli

- npm install --save-dev gulp
- npm install -g gulp-cli

* Extend our `default` task in each task runner to take two arguments from the command line, a log level and a message, and write them out to the console in the format “[$LOG_LEVEL] $MESSAGE”

#### Solution

grunt.registerTask('default',function(){
	var logLevel = grunt.option('logLevel') ||'logLevel';
	var message = grunt.option('message') ||'message';
	console.log('Grunt log:['+logLevel+']-'+message);
});	
 grunt --logLevel='Info' --message='test'

gulp.task('mytask', function(){
	console.log(process.argv);
	
	var logLevel = process.argv.indexOf("--loglevel");
	var message = process.argv.indexOf("--message");
	var loglevel1;
	var message1;
	if(logLevel>-1) {
		loglevel1 = process.argv[logLevel+1];
	} else {
		loglevel1="logLevel..";
	}
	
		if(message>-1) {
		message1 = process.argv[message+1];
	} else {
		message1 = "message";
	}
	console.log('Gulp log:['+loglevel1+']-'+message1);
});
 gulp mytask --loglevel "INFO" --message "test"

Exercise-2
---
* Install and configure the `grunt-postcss` plugin to run on your compiled Sass and call both `autoprefixer` and `cssnano`. For options see this [documentation page](https://www.npmjs.com/package/grunt-postcss).

#### Solution:
```
-  npm install --save-dev grunt-postcss
- In GruntFile.js
Section: grunt.initConfig

	postcss: {
		processors: [
		require('autoprefixer')({browsers: 'last 2 versions'}), // add vendor prefixes 
        require('cssnano')() // minify the result 
		]
	}
	


```

* Register a new task called `css` in your grunt file that first compiles your Sass and then runs the `postcss` command with your configuration setup above..
#### Solution:
```
Not sure on compiling saas
  //grunt-postcss should run autoprefixer and cssnano
  grunt.registerTask('postcss', function(){
	  grunt.task.run(['postcss']);
  });
```
* Create a `build` that runs our all our tasks so far (except `deploy`) such that when it’s completed you have a fully compiled and optimized set of code inside `dest/`. Then add `build` as the first task in `deploy` such that running `deploy` does everything you need to compile and ship your code.
#### Solution:
```

//'build' that runs all our tasks
grunt.registerTask('build', function(){
	  grunt.task.run(['cssmin', 'uglify', 'htmllint', 'csslint', 'jshint','postcss']);
  });
  
grunt.registerTask('deploy', function (releaseType) {
	 grunt.task.run(['build']);
    if (!releaseType) {
      releaseType = 'patch';
    }
    grunt.task.run(['version::' + releaseType, 'exec:add', 'exec:commit', 'exec:push']);
  });
```