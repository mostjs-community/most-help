- [ES6](#es6)
- [typescript](#typscript)
- [livescript](#livescript)

### `most@1.0.0 -> most@2.0.0`
*( explained by example in ES6, typescript and livescript )* 😀.


# `ES6`

#### Example 1

In our first example we are trying to create a `most` stream from `nodejs` infamous `fs.readFile` function, which accepts a callback as it's final argument.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```js
var fs = require("fs")
var most = require("most")
var most_create = require("@most/create")

$readFile = (filename) =>
{
  var cb = (add,end,error) =>
  {

    fs.readFile(filename,(err,data) =>
    {
      if(err) {error() return }
      add(data)
      end()
    })
  }
  return most_create(cb)
}

$readFile("package.json")
.drain()
```
</pre>
</td>
<td>
<pre>

```js
import * as M from "@most/core";
import { newDefaultScheduler } from "@most/scheduler";
import * as fs from "fs";

export const readFile = (path) =>
  M.newStream((sink, scheduler) => {
    fs.readFile(path, (err, data) => {
      if (err) {
        return sink.error(scheduler.currentTime(), err);
      }

      sink.event(scheduler.currentTime(), data);
    });

    return { dispose: () => {} };
  });

const stream = readFile("package.json");
const scheduler = newDefaultScheduler();

M.runEffects(M.tap(console.log, stream), scheduler);
```

https://codesandbox.io/s/wizardly-pasteur-sywhz

</pre>
</td>
</tr>
</table>


**Q.** would there be a practical difference between using `scheduler.asap()` instead of `scheduler.currentTime()` ?

**A.**

#### Example 2

In this example we are going to create a a subprocess `sh` shell inside our `node.js` app, and expose it as a `most.js` stream.

In `most@1.0.0` we employ a "hack" of using the `.unsubscribe` method to close our readline interface.

Apparantly in `most@2.0.0` you can do this more naturally, let's watch if it's possible 🤨.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```js
var most_create = require("@most/create").create

var cp = require("child_process")

var readline = require('readline')

wait = (t, f) => {return setTimeout(f, t)}

noop = () => {}

$bash = {}

$bash.out = most_create((add, end, error) =>
{

  var shell = cp.spawn('sh', [],{shell:true})

  shell.stdout.on('data',(data) => {add(data)})

  shell.stderr.on('data',(data) => {error(data)})

})

$bash.in = most_create((add, end, error) =>
{
  var rl = readline.createInterface({input: process.stdin});

  rl.on('line',(input) => {add(input)})

  return () => {return rl.close()};

})

$bash.in // stdin mostjs stream

$bash.out // stdout mostjs stream

sub = $bash.in.subscribe(noop)

// make sure to unsubscribe when you are done

wait(3000, () => return sub.unsubscribe())
```
</pre>
</td>
<td>
<pre>

```js
var most_create = require("@most/create").create

var cp = require("child_process")

var readline = require('readline')

```

</pre>
</td>
</tr>
</table>

# `TypeScript`

#### Example 1

In our first example we are trying to create a `most` stream from `nodejs` infamous `fs.readFile` function, which accepts a callback as it's final argument.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```typescript

```
</pre>
</td>
<td>
<pre>

```typescript

```
</pre>
</td>
</tr>
</table>


#### Example 2

In this example we are going to create a a subprocess `sh` shell inside our `node.js` app, and expose it as a `most.js` stream.

In `most@1.0.0` we employ a "hack" of using the `.unsubscribe` method to close our readline interface.

Apparantly in `most@2.0.0` you can do this more naturally, let's watch if it's possible 🤨.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```typescript

```
</pre>
</td>
<td>
<pre>

```typescript

```
</pre>
</td>
</tr>
</table>



# `LiveScript`

#### Example 1

In our first example we are trying to create a `most` stream from `nodejs` infamous `fs.readFile` function, which accepts a callback as it's final argument.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```livescript
fs = require \fs
most = require \most
most_create = require \@most/create

$readFile = (filename) ->

  (add,end,error) <- most_create
  (err,data) <- fs.readFile filename

  if err then error! return
    add data
    end!

$readFile \package.json
.drain!
```
</pre>
</td>
<td>
<pre>

```ls
M = require \@most/core

{ newDefaultScheduler } = require \@most/scheduler

fs = require \fs

readFile = (path) ->

  do
    (sink, scheduler) <- M.newStream

    (err, data) <- fs.readFile path

    if err
      return sink.error scheduler.currentTime!, err

    sink.event scheduler.currentTime!, data

    return { dispose: -> {} }

stream = readFile \package.json

scheduler = newDefaultScheduler!

M.runEffects M.tap(console.log, stream), scheduler

```

</pre>
</td>
</tr>
</table>

#### Example 2

In this example we are going to create a a subprocess `sh` shell inside our `node.js` app, and expose it as a `most.js` stream.

In `most@1.0.0` we employ a "hack" of using the `.unsubscribe` method to close our readline interface.

Apparantly in `most@2.0.0` you can do this more naturally, let's watch if it's possible 🤨.

<table>
<tr>
<th>most@1.0.0</th>
<th>most@2.0.0</th>
</tr>
<tr>
<td>
<pre>

```ls

most_create = (require "@most/create").create

cp = require "child_process"

readline = require 'readline'

wait = (t,f) -> setTimeout f,t

noop = !->

$bash = {}

$bash.out = most_create (add,end,error) !->

  shell = cp.spawn 'sh',[],{shell:true}

  shell.stdout.on \data,(data) !-> add data

  shell.stderr.on \data,(data) !-> error data


$bash.in = most_create (add,end,error) ->

  rl = readline.createInterface {input:process.stdin}

  rl.on \line,(input) -> add input

  -> rl.close!

sub = $bash.in.subscribe noop


do

  <- wait 3000

  sub.unsubscribe!

$bash.in # <-- stdin mostjs stream

$bash.out # <-- stdout mostjs stream

# make sure to unsubscribe when you are done
```
</pre>
</td>
<td>
<pre>

```ls
M = require \@most/core

{ newDefaultScheduler } require \@most/scheduler

fs = require \fs

readline = require \readline
```

</pre>
</td>
</tr>
</table>
