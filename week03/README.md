# My Notes - Week 3

## Schema Design

## Aggregation Framework

## `findAndModify`

## Map Reduce
* Create a map function
* Test in the shell `doc = db.foo.findOne(); m.apply( doc )`
* return output inline: `res = db.things.mapReduce(m, r, { out: { inline: 1 }})`
* `mapReduce` can be executed as multiple reductions in sharded environments, which is awesome

## Homeworks

### 3.1

    db.zips.aggregate([
      { $group:{ _id: "$state", count: { $sum: 1 } } }, 
      { $sort: { count: -1 } }, 
      { $limit: 4 }
    ])
    
### 3.2

    db.zips.aggregate([
      { $project: { _id: { $substr: ["$_id", 0, 1] } } },
      { $group: { _id: "$_id", n: { $sum: 1 } } }
    ])
    
    db.zips.aggregate([
      { $project: { _id: { $substr: ["$city", 0, 1] } } }, 
      { $group: { _id: "$_id", n: { $sum: 1 }}} 
    ])
    
    db.zips.remove({ city: /^[0-9]/})
    
    db.zips.count()
    
### 3.3

### 3.4
    
    var reduce_closest = function(city, counts){  
      return Array.sum(counts); 
    }
    
    db.zips.mapReduce(
      map_closest, 
      reduce_closest, 
      { 
        query: { state: 'PA' }, 
        out: { inline: 1 } 
      }
    )