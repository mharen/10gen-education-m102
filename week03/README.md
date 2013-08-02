# My Notes - Week 3

## Schema Design

## Aggregation Framework

## `findAndModify`

## Map Reduce
* Create a map function
* Test in the shell `doc = db.foo.findOne(); m.apply( doc )`
* return output inline: `res = db.things.mapReduce(m, r, { out: { inline: 1 }})`
* `mapReduce` can be executed as multiple reductions in sharded environments, which is awesome
