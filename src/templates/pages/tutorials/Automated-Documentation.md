---
published: false
---
## Overview

Using a simple Grunt.js workflow, we will:

1. Run the [grunt-github-api](https://github.com/JeffHerb/grunt-github-api/) task to pull down and agreggate README content _for a number of repositories at once_ from GitHub's API
2. Run the [Assemble](http://assemble.io) task to "distill", organize and transform the README content into stylish HTML documentation _that is better than the sum of its parts_.


### The Gruntfile

```js
module.exports = function(grunt) {
  grunt.initConfig({

    github: {
      options: {
        filters: {'type': 'public'}
      },
      readme: {
        src: '/repos/assemble/assemble/readme',
        dest: 'readme.json'
      }
    },

    // Templates
    assemble: {
      readme: {
        options: {
          helpers: 'helper-decode.js'
        },
        src: ['index.hbs'],
        dest: 'index.html'
      }
    }

  });

  // Load npm and local plugins.
  grunt.loadNpmTasks('assemble');
  grunt.loadNpmTasks('grunt-github-api');

  // Default task to be run.
  grunt.registerTask('default', ['github', 'assemble']);
};
```

#### 1. Uses `github` task to get "content"

GitHub's API (as you know) allows you to retrieve the contents of files in a repo as base64 formatted content.

#### 2. Decodes base64 to utf8

So I created this basic helper to decode the base64 content returned from the API:

```js
Handlebars.registerHelper('decode', function(encoded) {
  return new Handlebars.SafeString(new Buffer(encoded || '', 'base64').toString('utf8'));
});
```


## Who would use this?

Mostly big projects, or projects that are planning for bigness.

Designers or developers who:
* manage documentation for more than one GitHub project
* manage lots of documentation that is spread across multiple repos
* Already use Grunt.js (or Assemble) on their projectsl

## Why do this?

If we can make this process smooth enough, fast enough, and with very little to _zero_ abstraction between the content, metadata and the documentation that is generated by this process, then in theory this will not only ease the burden of maintaining documentation for users or organizations with large numbers of repositories, but it will also encourage new and interesting ways of maintaining documentation that were previously impractical or even undesireable but are now more effective than current methods.

For example:

* Frameworks consisting of components, each with separate repositories, can easily allow each component to maintain its own documentation (for users of the component) while also easily aggregating and organizing the documentation (for users of the framework)

 new make it easier for GitHub users or organizations to maintain documentation.

economies of scope and scale
Challenges:

* **One README per target**. As it stands, the [grunt-github-api](https://github.com/JeffHerb/grunt-github-api/) task requires a separate target for each README we wish to pull down. We will need to make some adjustments to allow more than one readme per target
* **base64 encoding**: content is returned from GitHub with `base64` encoding. We need to convert this to `utf8`.
* **Very little metadata**. When we pull down the


markdown-formatted
decode
 we will automate the process of , and then documentations from and converting and generating This is the basic workflow that I'm using to create a proof of concept for automating the process of generating documentation from lots of repos at once.



# Overview


We use two (maybe three) Grunt tasks to automate this entire process:

* first we



*

and this template:

```html
\{{decode readme.content}}
```
####

At present, the way the proof-of concept is setup when you run `grunt`, the `github` task just pulls down a specified readme from GitHub, and then Assemble decodes it and converts the decoded markdown into HTML. It works nicely, the entire process just takes a couple of seconds, and when I pull down 5 or 6 readme's there is very little impact on the total time - since the initial handshake takes most of the time.

 So it's only kind-of, sort of, almost really cool.

- using the "github" task you created, a user can pull down README content for an arbitrary number of repos from GitHub's API
- next, using the repo name as the uniqe id, any package.json files are matched up with the content re for metadata
- and a "matching" JSON object content along with the and content from  I created some helpers and templates that use the metadata documentation for a repo or even a collection of repos,