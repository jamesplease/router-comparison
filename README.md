# router-comparison

A comparison of Router implementations.

### About

Routers on the client play a major part in modern web apps. For many apps, the core logic of application state
is encoded in the router.

However, each popular library has its own implementation that differs from the others. I made this chart
to compare them.

A description of each feature can be found below the table.

Feature             | Backbone | Ember | React | UI-Router | Stateman | Angular v2 | StateRouter
------------------- | -------- | ----- | ----- | --------- | -------- | ---------- | -----------
DSL                 | ✔        | ✔     | ✔     | ✔         | ✔        | ✔          | ✔
Asynchronous        | ✘*       | ✔     | ✔     | ✔         | ✔        | ✔          | ✔
Sort: specificity   | ✘        | ✔     | ?     | ✘         | ✘        | ?          | ✔
Sort: order added   | ✔        | ✘     | ?     | ✔         | ✔        | ?          | ✘
Regex for `:dynamic`| ✘        | ✘     | ?     | ?         | ✔        | ?          | ✘  
Nested routes       | ✘        | ✔     | ✔     | ✔         | ✔        | ✔          | ✔
Reusable routes     | ✘        | ✔     | ?     | ?         | ?        | ?          | ✘  
Splats              | ✔        | ✔     | ?     | ?         | ✘        | ?          | ✔
Regex instead of DSL| ✔        | ?     | ?     | ?         | ?        | ?          | ✘
Optional history    | ✘        | ✔     | ?     | ?         | ✘        | ?          | ✔
Includes history    | ✔        | ✔     | ?     | ?         | ✘        | ?          | ✘
Cancel navigation   | ✘*       | ✔     | ?     | ?         | ?        | ✔          | ✔
Redirect navigation | ✔        | ✔     | ✔     | ?         | ?        | ✔          | ✔
`.active` links     | ✘        | ✔     | ✔     | ?         | ?        | ?          | ✘
Query params        | ?        | ?     | ?     | ?         | ?        | ?          | ?
'Index' route       | ?        | ✔     | ✔     | ?         | ?        | ?          | ?

\* Refer to the corresponding section below to see notes.

### Features

#### DSL

A DSL is a domain specific language. If you haven't heard that term before, don't worry: it's simpler than it
sounds. An example of a DSL are the templating languages we use to output HTML, like Handlebars, Underscore,
or Angular templates.

These features are like mini programming languages that are designed to do one task.

In Routers, the DSL is the syntax you use to specify what sorts of URLs each route matches. For instance, `books/:bookId`
is an example of a DSL, where the `:` designates the start of a dynamic URL segment.

#### Asynchronous



### Omissions

I omitted certain routers from the above chart.

#### Ampersand.js

Too similar to Backbone's.

#### Marionette.js

Too similar to Backbone's.

#### Angular.js v1

Not very notable, and in the process of being completely replaced in Angular v2.
