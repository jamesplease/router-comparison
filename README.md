# router-comparison

A comparison of Router implementations.

### About

Routers on the client play a major part in modern web apps. For many apps, the core logic of where the user is
in the application is encoded in the router.

However, each popular library has its own implementation that differs from the others. I made this chart
to compare them.

A description of each feature can be found below the table.

Feature             | Backbone | Ember | React | UI-Router | Stateman | StateRouter
------------------- | -------- | ----- | ----- | --------- | -------- | -----------
DSL                 | ✔        | ✔     | ✔     | ✔         | ✔        | ✔
Regex for `:dynamic`| ✘        | ✘     | ✘     | ✔         | ✔        | ✘
Splats              | ✔        | ✔     | ✔     | ✔         | ✘        | ✔
Optional segments   | ✔        | ✘     | ✔     | ✘*        | ✘*       | ✘
404 abstraction     | ✘        | ✘     | ✔     | ✔         | ✔        | ✘
Regex instead of DSL| ✔        | ✘     | ✘     | ✘         | ✘        | ✘
Asynchronous        | ✘*       | ✔     | ✔     | ✔         | ✔        | ✔
Sort: specificity   | ✘        | ✔     | ?     | ✘         | ✘        | ✔
Sort: order added   | ✔        | ✘     | ?     | ✔         | ✔        | ✘ 
Nested routes       | ✘        | ✔     | ✔     | ✔         | ✔        | ✔
Reusable routes     | ✘        | ✔     | ?     | ?         | ?        | ✘  
Optional history    | ✘        | ✔     | ✘     | ✔         | ✘        | ✔
Includes history    | ✔        | ✔     | ✔     | ?         | ✔        | ✘
Cancel navigation   | ✘*       | ✔     | ✔     | ✔         | ✔        | ✔
Redirect navigation | ✔        | ✔     | ✔     | ✔*        | ✔        | ✔
`.active` links     | ✘        | ✔     | ✔     | ✘         | ✘        | ✘
Template links      | ✘        | ✔     | ✔     | ✔         | ✘        | ✘
Query params        | ✘        | ✔     | ✔     | ✔         | ✔        | ✔
'Index' states      | ✘        | ✔     | ✔     | ✘         | ✘        | ✔ 
'Loading' states    | ✘        | ✔     | ✘     | ✘         | ✘        | ✔
'Error' states      | ✘        | ✔     | ✘     | ✘*        | ✘*       | ✔
'Abstract' states   | ✘        | ✘     | ✘     | ✔         | ✘        | ✘
Pubsub              | ✔        | ✘     | ✘     | ✔         | ✔        | ✘
Scrolling           | ✘        | ✘     | ✔     | ✘*        | ✘        | ?
Group data fetching | ✘        | ✔     | ✔*    | ✘         | ✘        | ✘ 

\* Refer to the corresponding section below to see notes.

### Features

#### DSL

A DSL is a domain specific language. If you haven't heard that term before, don't worry: it's simpler than it
sounds. An example of a DSL are the templating languages we use to output HTML, like Handlebars, Underscore,
or Angular templates.

These features are like mini programming languages that are designed to do one task.

In Routers, the DSL is the syntax you use to specify what sorts of URLs each route matches. For instance, `books/:bookId`
is an example of a DSL, where the `:` designates the start of a dynamic URL segment.

Every router comes with a DSL, and they share a common set of features. However, there are differences. The
next few features in this list cover those differences.

#### Regex for `:dynamic`

Dynamic parts capture a single URL segment of a path. A URL segment are each of the sections separated by `/`. So,
for instance, in `/books/2`, there are two segments: `books` and `2`.

Generally, dynamic segments capture **anything** between the parenthesis. But some libraries allow you to limit
what's matched by a dynamic segment with a regex or type annotation.

UI-Router is the first library to do this. Because Stateman is based off of UI-Router, it, too, has a similar feature.

An example in UI-Router is:

```js
"/user/{id:[0-9a-fA-F]{1,8}}""
```

#### Splats

Splats can be used to capture more than one segment of the URL, and are a denoted by a `*`. An example is:

`"/users/*splat"`

This would match, for instance, `/users/james/picture`.

Splats are interesting because of the way different routers treat them. In Backbone and Ember, they're of particular importance,
because they're the way that you specify 404 routes.

`"*notFound"`

This works really well for Ember, and is terrible for Backbone. The reason is because Ember has a unique matching algorithm:
less specific routes are matched last. So even if you specify the 404 route first, it won't be matched unless nothing else does.

Backbone isn't so smart. It matches on a first come, first serve basis, which requires you to place the 404 route last. In practice,
this can end up being a tedious requirement to fulfill.

Other libraries use one of two other abstractions for routes: either a special named route, and/or a pubsub event that is fired on the
router itself.

#### Optional segments

Optional segments are, well, exactly what you'd expect. Segments that don't need to be there. They're generally designated by a `?` or
parentheses.

An example from Backbone, which uses parentheses:

`docs/:section(/:subsection)`

#### 404 abstraction

A 404 abstraction is anything other than splats to designate not found pages. The alternatives are:

1. A specially named Route (React Router, Stateman)
2. Pubsub events (UI-Router, Stateman)

#### Regex instead of DSL

Backbone lets you write a regular expression instead of the DSL. As you would expect, this gets ugly quickly. Because of the small
amount of code necessary to implement the feature, it does fit in with Backbone's minimalist ideas. It's comparable, I think, to
allowing regex in the dynamic segments.

#### Asynchronous

Whether or not the route handlers are asynchronous. This is important for routers that support nesting, because you'll often
be fetching data in your handlers.

Backbone is listed as not supporting this, but I want to clarify what I mean. Because Backbone's handlers are independent,
it simply doesn't matter if your callback is asynchronous or synchronous: nothing depends on them (out of the box).

#### Sort: specificity

Route resolution is an important component of any router. If the user has two routes:

`books/:bookId` and `books/author`

Which will match when the user navigates to `books/author`? Does it depend on the order
that they are added?

For most routers, the answer to the above question is **yes**. `books/author` will
need to be added to the router before `books/:bookId`, or else the dynamic segment
will match the string.

For routers that support regex for dynamic segments, this matters less because that provides
a way to make your dynamic segments even more specific.

Ember is the only router that I know of to support the proper algorithm to support this.

#### Sort: order added

All other routers sort by the order added. Therefore, you must specify your dynamic segments and
splats last, or use regexes to restrict what your dynamic segments match.

#### Nested routes

Are the routes related? Does entering a child route cause you to first enter the parent route? If yes,
then it's a nested router.

Given that routes encode your app's *location* (which is often represented by a URL), it's no surprise
that most routers these days are nested.

Backbone is the exception here.

#### Reusable routes

#### Optional history

#### Includes history

#### Cancel navigation

#### Redirect navigation

#### `.active` links

#### Query params

#### `Index` routes

### Omissions

I omitted certain routers from the above chart.

#### Ampersand.js

Too similar to Backbone's.

#### Marionette.js

Too similar to Backbone's.

#### Angular.js v1

Not very notable, and in the process of being completely replaced in Angular v2.
