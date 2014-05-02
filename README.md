# Monkey Documentation Project

Monkey is a fully featured HTTP Server for Linux. The present repository contains the official documentation for each Monkey version covering admistration topics and general tutorials to accomplish specific setups.

## How the documentation is built

All documentation is made in GIT Flavored Markdown format and is rendered using the [Gitbook](https://github.com/GitbookIO/gitbook) tool. To generate a HTML documenation from this source please issue the following command:

```shell
$ gitbook serve -p 9000 src/
```

Then point your browser at [http://127.0.0.1:9000/] to see the HTML documents. For more options check the Gitbook help.
