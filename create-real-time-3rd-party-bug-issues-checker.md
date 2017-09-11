# Create Real Time 3rd Party Bug Issue Checker

### Abstract
Developing high quality cross-browser web applications is challenging, there are different JavaScript engines in various browsers running in mobile phones and desktop computers. In a rapid development cycle, using third party libraries to support development is necessary. At the same time, using older versions of third libraries is common because updating to the latest libraries is time-consuming for code change and QA testing. So, in a development cycle, providing bug issues relative to the methods that developers use in those third party libraries is to reduce existing bugs found by the communities using the libraries. Many developers use methods from third party libraries without paying attention to its bug issues and release notes until they have bugs to fix.

In web development, Grunt/Gulp are common tools to provide automation features that help developers do faster development. In this paper, I will cover current work done on how to create a Grunt/Gulp plugin to be able to check bug issues in the methods used in third party libraries. And I will run the task in grunt-watch/gulp-watch in order to provide real time update. When developers use third party library methods in applications, they will get instant feedback for what happened in the current issue associated with the methods used. Using this plugin is to provide another view to avoid known bugs before QA tests and increase code quality by using third party libraries.

### 1. Introduction

Developers will be more valuable if they can be effective in writing high-quality codes. To improve developers with using third party libraries, it’s an important to know what issues for the methods used in libraries. Since no many libraries will document the issues of methods in API documentation, most developers won’t pay attentions until issues come up. In order to educate and warn developers while using problematic methods during code run time, it need a tool to analyze codes and provide feedbacks of methods with current issues documented for libraries.

The idea of tool is to build a Grunt/Gulp plugin that will communicate with library’s hosting server API and provide feedback for open bug issues. The paper is focused on building a Grunt plugin because my current team is using Grunt and building the Grunt plugin could be tested in our current project environment.

### 2. Desgin for Issue Checker Plugin
The Grunt is managed via node JS package manage (NPM), a Grunt plugin not only uses Grunt API, but also it can inject outside node JS libraries through NPM. Each plugin has its task configuration for defining default options. The configuration options for Issue Checker plugin are required to provide sources path for issue checking, and host server URLs with third party library. Also, it accepts optional settings, such as cache file path. After setting up Grunt task configuration and load Grunt plugins. Next, we can execute tasks in our command-line interface. There are five steps running sequentially in the plugin: generate request, process request, write response, match methods and show results.

1. Generate request: get URLs of third library API from configuration and use default file path for cache API results if it’s no configured.
2. Process request: check whether cache file is existent; if cache file is over 24 hours old, remove cache file and make HTTP requests to fetch the latest open issues, otherwise, read cache file and return a promise.
3. Write response: when promise is finished, parse the response and save as JavaScript object in key/value pair format. If responses are from HTTP, store the object in a new cache file and save it locally.
4. Match methods: loop through files in source directories to filter out methods used in third party library, and match it with key in JavaScript Object and return issue information to next step.
5. Show results: display results in a simple and colorful table format.

The plugin could run as standalone task in Grunt. However, it can run as a sub task in Grunt watch task to provide instant response to current changed source files; it’s best practice to do because it will increase performance by searching methods in small sized files instead of all source codes.
