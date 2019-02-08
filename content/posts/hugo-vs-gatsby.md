---
title: "Hugo vs Gatsby"
date: 2019-02-08T12:24:32+03:00
author: "Vance Longwill"
draft: false
---

### Static Site Generators - [Hugo](http://gohugo.io/) vs [Gatsby](http://gatsbyjs.org/)

To launch myself as a freelance web developer, I needed a personal site where I could share my portfolio and introduce myself to potential clients. Coming from a react/node.js background the obvious tool for me to choose in this scenario would definitely be [Gatsby](http://gatsbyjs.org/), a static site generator built on top of react. However, since experiencing some heavy **javascript fatigue**, I've been searching for a new direction. In spite of all the hype, I eventually found working with react and modern javascript to be repetitive and tiring. Stitching other peoples libraries together into a frankenstein's monster of an app was fun at first, but it grew old very quickly. The problem is that it's just not mentally stimulating. Managing state and redux and component heirarchies does sound cool, but I couldn't help feeling it was completely unnecessary. In my opinion, the javascript revolution has totally fucked up the internet. HTML/CSS are perfectly valid tools for layout and design, and barring sites with a lot of application state, then that's what you should use. Single page applications are cool but so is simplicity.

In an effort to transition myself to a more backend-heavy role, I've been learning [Golang](https://golang.org). It's a wonderfully simple language which manages to be everything that ES6+/node is not. First of all, it's fast, almost as fast as C. Go's speed is already much-talked-about, so I'll move on. The second thing I liked was the syntax and language design. Javascript's usage scope has grown exponentially over the years and to mitigate this, more and more features have been added to the language. Callbacks, promises, async/await, classes, destructuring, arrow functions and the like have complicated a language which was originally intended for much more basic uses. The bloat is tangeable, and it's very hard to keep track of exactly what's going on under the hood, behind all the syntactic sugar. I couldn't believe my ears when I heard that Go avoids even the added complexity of convenience methods like map/reduce/filter. All looping is performed with for loops. Complex async logic, for example handling multiple race conditions, might require an additional library like redux-saga in Javascript, but can be simply expressed in vanilla go with features such as channels and select. The downside of all these external dependencies in my experience, is that they force you into a corner. Many parts of your application must be compatible with whatever convoluted architecture the library is imposing on you. Go was a refreshing change of scene for me. 

In this example you can compare two styles of managing async race conditions. The first, using the redux-saga library and the second using standard features of go. Although the context in these examples is very different, I think it shows how much of a mess the javascript world has become.

```js
import { race, call, put, delay } from 'redux-saga/effects'

function* fetchPostsWithTimeout() {
  const {posts, timeout} = yield race({
    posts: delay(500)
    timeout: delay(1000)
  })

  if (posts)
    yield put({type: 'POSTS_RECEIVED'})
  else
    yield put({type: 'TIMEOUT_ERROR'})
}
```

```go
package main

import "time"
import "fmt"

func main() {

    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        c1 <- "received some posts"
    }()

    go func() {
        time.Sleep(2 * time.Second)
        c2 <- "timeout error"
    }()

    select {
    case msg1 := <-c1:
        fmt.Println(msg1)
    case msg2 := <-c2:
        fmt.Println(msg2)
    }
}
```


So I looked into another static site generator which would spare me from more javascript fatigue. Hugo is written in go and has a nice community with a fair offerring of themes and starters. Unlike Gatsby, it doesn't overcomplicate things and it doesn't have millions of dependencies. Instead, like Go itself, it lives on your hard drive as a single executable, does exactly what it needs to a nothing more. As a result, it's not only fast, but also easy to use.

On the road, I don't always have a decent internet connection, so waiting for lots of node modules to download and using up all my data allowance is not appealing. This might not seem like a big deal if you're lucky enough to enjoy a consistent internet connection, but when you want to rebuild your site on a cloud based CI/CD platform, it might be a pain. To illustrate the difference in bloat between th two static site generators, I timed how long it takes to setup a new project with each tool.

1. Gatsby
  ```
  [~/projects]$ time gatsby new mysite
  ...
  gatsby new mysite  36.78s user 30.03s system 113% cpu 58.637 total
  ```
2. Hugo
```
[~/projects]$ time hugo new site mysite
...
hugo new site mysite  0.03s user 0.01s system 102% cpu 0.048 total
```

As you can see above, generating a new project with gatsby took just under a minute, whereas with hugo it took just under a 20th of a second. While true that gatsby's execution time can vary greatly depending on the internet connection, cacheing policy, and much more, it's obvious which tool comes out top here. If like me, you're aiming to write a simple blog/personal site, then there's no real need for javascript, let alone a jacked-up generator like gatsby.

One clear win for gatsby might be in the commercial app space. Many e-commerce sites, including ones I've worken on, have pages and pages of static content like about pages and marketing campaigns living inside a react app. This happens for no other reason than convenience. There's little to no interactivity on those pages, but the user is forced to download kilobytes of javascript just to read some terms & conditions. It takes a lot of technical nouse to workaround this issue with SSR, cacheing and other voodoo. Gatsby automates this kind of thing and offers a host of plugins to fill in the blanks, making it much easier and more maintainable than building a custom solution from scratch.

For a personal site however, it was more important to me that I could easily publish and customise.  In the end I forked one of the great [hugo themes](https://themes.gohugo.io), [terrassa](https://themes.gohugo.io/hugo-terrassa-theme/). Hugo's community provided a decent number of high quality themes, which are easily extendible thanks to their standard layout. Comparing with Gatsby's themes, I think that Hugo's are much more practical, and more polished.

All said and done, in terms of picking a clear winner, it's just too close to call. For small blogs, personal sites, landing pages and similar projects, I'd definitely choose Hugo. Anything more interactive and stateful might benefit from the component based architecture in React and Gatsby.

___ 

This site was build with Hugo, you can find the source code [here](https://github.com/VanceLongwill/vancelongwill.github.io).

