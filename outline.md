# Break Up with Your Build Tools

![Breaking Up](/images/breaking-up.gif)


# What are you using?


# Javascript Build Tools
  ## Gulp
  ## Grunt
  ## Brunch
  ## ...

  [JSJ - JavaScript Tools Fatigue](https://devchat.tv/js-jabber/194-jsj-javascript-tools-fatigue)
  [State of JavaScript Build Tools](https://gist.github.com/callumacrae/9231589)


# What's the problem?
  - Over reliance on plugins <!-- .element: class="fragment" -->
  - Out of date dependencies <!-- .element: class="fragment" -->
  - One more API to remember &amp; debug <!-- .element: class="fragment" -->


# Some advantages of NPM
  - Running different build tools from NPM <!-- .element: class="fragment" -->
  - Expected default for NodeJS apps <!-- .element: class="fragment" -->
  - Standardization across projects <!-- .element: class="fragment" -->
  - You already know it <!-- .element: class="fragment" -->


# package.json
```
{
  "name": "mySecretApp",
  "version": "1.0.0",
  "description": "This solves all of the worlds problems",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Rob Tarr",
  "license": "ISC"
}
```


# Details before we get going
  ## default named tasks
  - start
  - test

Note: For things like Heroku, CircleCI, etc. they will look for
these defaults.


## pre/post tasks
```
scripts: {
  "prestart": "...",
  "start": "...",
  "poststart": "..."
}
```

```
scripts: {
  "premyTask": "...",
  "myTask": "...",
  "postmyTask": "..."
}
```
 <!-- .element: class="fragment" -->


## Access to `node_modules/.bin`
```
"scripts": {
  "test": "node_modules/.bin/yourFancyNodeModule
}
```
## =
```
"scripts": {
  "test": "yourFancyNodeModule"
}
```


## Access to env vars
```
"scripts": {
  "env": "env"
}
```


# What Can I Run?
  - CLI Tools <!-- .element: class="fragment" -->
  - Bash scripts <!-- .element: class="fragment" -->
  - JavaScript files <!-- .element: class="fragment" -->
  - Other build tools?!?! <!-- .element: class="fragment" -->


# How Do You Do That with NPM?


# Watch scripts
  ## Use what's already there

  ```
  "scripts": {
    "start": "webpack -w"
  }
  ```


![Charizard](http://orig01.deviantart.net/b6b5/f/2014/130/7/1/charizard_by_killian_griffin-d7hx9rp.gif) <!-- .element: style="width: 50%;" -->


![Chokidar](/images/chokidar.jpg)


# Custom Watch Scripts

watch.js
```
var chokidar = require('chokidar');

chokidar.watch('./data').on('all', (event, path) => {
  // process data files
});

chokidar.watch('./templates').on('all', (event, path) => {
  // compile HTML
});
```

package.json
```
"scripts": {
  "start": "node scripts/watch.js"
}
```


# Concat your files
```
"scripts": {
  "build:js": "cat *.js > app.js"
}
```


![Kramer Driving Fire Truck](/images/kramer-fire-truck.gif) <!-- .element: style="width: 50%;" -->


# Concat your files
```
"scripts": {
  "start": "webpack"
}
```


# Run your tests
```
"scripts": {
  "test": "jasmine-node specs/*.js"
}
```


# Composing Commands
```
"scripts": {
  "start": "webpack -p && npm run scss && node app.js"
}
```


#Pre & Post
```
"scripts": {
  "pre": "npm run scss",
  "start": "webpack -p",
  "post": "node app.js"
}
```


# Composing Commands Concurrently
```
"scripts": {
  "start": "concurrently 'webpack -w' 'npm run scss' 'nodemon app.js' 'npm run test'"
  "test": "jasmine-node specs/*.js --autotest --watch specs"
}
```


# ENV specific tasks
```
"scripts": {
  "start": "node scripts/start.js"
}
```

```
let env = process.env.NODE_ENV;
const shell = require('shelljs');

if (env === 'production') {
  console.log('Starting Production...');
  shell.exec('webpack -p && npm run scss && node app.js');
} else {
  console.log('Starting Development...');
  shell.exec(`concurrently 'webpack -w' 'npm run scss' 'nodemon app.js' 'npm run test'`);
}
```

NOTE: Sure, this adds a small amoutn of complexity, but it's complexity
that is entirely visible and familiar to me.


# Passing Parameters

```
"scripts": {
  "test": "jasmine-node specs/*.js"
  "test:xunit": "npm run test -- --reporter xunit"
}
```


# Fingerprint Your Files
```
"scripts": {
  "build": "webpack -p | hashmark 'assets/bundle.{hash}.js'"
  "postbuild": "< ./assets/bundle.js hashmark './assets/bundle.{hash}.js'"
}
```

```bash
assets/
  bundle.46732501c8b3255c87f0107620284485ec0095221017f194e3495abbd2f6b16a.js
```

[hashmark](https://github.com/keithamus/hashmark)


# Organize Your Tasks
```
"scripts": {
  "test": "scripty",
  "test:unit": "scripty",
  "test:integration": "scripty"
}
```

[Scripty](https://github.com/testdouble/scripty#scripty)


# But wait, I use Windows?!?
  [Bash knows Windows](http://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/)


# GUI Tools


# NPM Scripts GUI
![npm-scripts-gui.png](/images/npm-scripts-gui.png)


# Site Ignite
![site-ignite.png](/images/site-ignite.png)


# Site Ignite
![site-ignite-running.png](/images/site-ignite-running.png)


## Reference
  - https://docs.npmjs.com/misc/scripts
  - http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/


## Let's try this out
