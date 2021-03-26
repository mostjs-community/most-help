
### `most@1.0.0 -> most@2.0.0`
*( explained by example ).*

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

$readFile = function(filename)
{
  var cb = function (add,end,error)
  {

    fs.readFile(filename,function(err,data){

      if(err) {error() return }

      add(data)

      })
  }
  return most_create(cb);
}

$readFile ("package.json")
.drain()
```
</pre>
</td>
<td>

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

</td>
</tr>
</table>
