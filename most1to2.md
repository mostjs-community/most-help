
### `most@1.0.0 -> most@2.0.0`
*( explained by example ).*

In our first example we are trying to create a `most` stream from `nodejs` infamous `fs.readFile` function, which accepts a callback as it's final argument.

```js

// done in most@1.0.0

var fs = require("fs")

var most = require("most")

var most_create = require("@most/create")

$readFile = function(filename)
{

  var cb = function (add,end,error)
  {

    fs.readFile(filename,function(err,data){

      if(err)
      {
        error()
        return
      }

      add(data)


      })
  }

  return most_create(cb);

}

```


```js
// done in most@2.0.0

```js
