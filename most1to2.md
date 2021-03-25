
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
// done in most@2.0.0

```

</td>
</tr>
</table>