# Gremlin Katas
Some katas to help practice CRUD operations in Gremlin

PRs welcome to improve these katas.

# Setup
In practicing these katas I've been using the Gremlin REPL that comes
with the standard installation of [Titan 1.0.0](http://thinkaurelius.github.io/titan/)

Have the Gremlin REPL running in a terminal.

# Usage
Scroll this Readme to each kata. Do the kata in the REPL. Check the answer. Repeat.

# Katas

---

## Kata \#0 - Gremlin/Groovy REPL commands

### Load the Sugar plugin.

```
SugarLoader.load()
```

---

## Kata \#1 - Basic CRUD operations

One or more of the CRUD letters in front of the kata will indicate what is being practiced.

### Set the variable `graph` to an instance of the `Modern` graph and a `TraversalSource` instance of that graph to the variable `g`.

```
graph = TinkerFactory.createModern()
g = graph.traversal(standard())
```

The graph that you have just loaded looks like this:

![Modern Graph](https://academy.datastax.com/sites/default/files/tinkerpop-modern.png)

### (R) List the edges and vertices in the graph.

```
g.E()
g.V()
```

### (R) Print the number of edges and vertices in this graph.

```
g.E().count()
g.V().count()
```

### (R) Find the first vertex.

```
g.V(1)
or
g.V().next()
```

### (R) Find the first 2 vertices.

```
g.V().range(0, 2)
```

### (R) List all the keys and values for the vertices and edges.

```
g.V().valueMap()
g.E().valueMap()
```

### (R) List all the vertices that have the "person" `label`.

```
g.V().hasLabel('person')
or
g.V().has(label, 'person')
```

### (R) Get the vertex with `id` 1.

```
g.V().hasId(1)
or
g.V().has(id, 1)
```

### (R) Find the vertex with the name 'marko'.

```
g.V().has('name', 'marko')
```

### (R) Print the values that this vertex has.

```
g.V().has('name', 'marko').values()
```

### (R) List the out-going edges of this vertex.

```
g.V().has('name', 'marko').outE()
```

### (R) List the out-going edges of people that marko knows.

```
g.V().has('name', 'marko').outE('knows')
```

### (R) List the vertices on the other side of those edges.

```
g.V().has('name', 'marko').outE('knows').inV()
```

### (R) List the names associated with the vertices on the other side of those edges.

```
g.V().has('name', 'marko').outE('knows').inV().values('name')
```

### (R) Shorten the `outE('knows').inV()` part of the previous statement to a single command.

```
g.V().has('name', 'marko').out('knows').values('name')
```

### (R) List all the out/in/both edges with a 'knows' label.

```
g.V().outE('knows')
g.V().inE('knows')
g.V().bothE('knows')
```

### (R) Assign the result from `g.V().has('name', 'marko')` to a variable named marko.

```
marko = g.V().has('name', 'marko').next()
```

Why `next()`? `V()` returns an iterator that will return the first element when `next()` is called.

### (R) Find the `id/label` associated with the vertex (variable) `marko`.

```
g.V(marko).id()
g.V(marko).label()
```

### (R) Find who Marko knows that has an age of 32 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', 32)
```

### (R) Find who Marko knows that has an age greater than 30 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', gt(30))
```

### (R) Show the names of who Marko knows that has an age greater than 30 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', gt(30)).values('name')
```

### (CR) Check how many vertices there are in the graph. Create a new `person` vertex with the name "john" and an age of 40. Confirm that there is now one more vertex in the graph.

```
g.V().count()
g.addV(label, 'person', 'name', 'john', 'age', 40)
g.V().count()
```

### (CR) Check how many edges there are. Create a new "knows" edge from john to marko. Confirm that there is one more edge.

```
g.E().count()

// #1 verbose
g.V().hasId(12).next().addEdge('knows', g.V().hasId(1).next())

// #2 simplifying with variables
john = g.V().hasId(12).next()
// or
john = g.V().has('name', 'john').next()
john.addEdge('knows', marko)

g.E().count()
```

Note that your id for John (12) might be different.

### (R) Set the "lop" software vertex to the variable `lop`.

```
lop = g.V().has('name', 'lop').next()
```

### (C) Assign the john vertex to variable `john`. Add a "created" edge from vertices john to lop and assign that to the variable `c`.

```
john = g.V().has('name', 'john').next()
c = john.addEdge('created', lop)
```

### (R) List the names of everyone who created software lop.

```
g.V(lop).in('created').values('name')
```

### (R) List the `valueMap` for all of the `created` incoming edges to lop.

```
g.V(lop).inE('created').valueMap()
```

### (U) Update the `created` edge from `john` to `lop` with a weight of 0.1

```
g.V(john).outE('created').property('weight', 0.1)
```

### (RD) Determine the number of vertices. Delete the vertex "marko" using the `marko` variable. Confirm that there is one less vertex.

```
g.V().count()
g.V(marko).drop()
g.V().count()
```

### (D) Determine the number of edges. Remove the rest of the vertices. Confirm that there are no vertices or edges.

```
g.E().count()
g.V().drop()
g.V().count()
g.E().count()
```

---

## Kata \#2

CRUD operations specified in parens before each kata.

### Set variable `graph` and `g` to a new empty graph and `TraversalSource`.

```
graph = TinkerGraph.open()
g = graph.traversal(standard())
```

### (C) Add a new vertex with label of 'person', a name of 'sheldon' and an age of 33.

```
g.addV(label, 'person', 'name', 'sheldon', 'age', 33)
or
graph.addVertex(...)
```

### (R) Find the id of the vertex that was just added.

```
g.V().id()
Or
g.V().next().id()
```

### (C) Add a new vertex with label of 'person', a name of 'amy' and an age of 31.

```
g.addV(label, 'person', 'name', 'amy', 'age', 31)
```

### (CR) Add an edge from `sheldon` to `amy` with a label of dating. (Assign the shelond/amy vertices to the variables `sheldon` and `amy`. Assign the edge to the variable `relationship`.)

```
sheldon = g.V().has('name', 'sheldon').next()
amy = g.V().has('name', 'amy').next()
relationship = sheldon.addEdge('dating', amy)
```

### (C) Add a `person` vertex with the name `leonard`, an age of `32` and a profession of `Experimental Physicist`. Assign this to variable `leonard`.

```
leonard = g.addV(label, 'person', 'name', 'leonard', 'age', 32, 'profession',
  'Experimental Physicist').next()
```

### (U) Update Sheldon to be a `Theoretical Physicist`

```
sheldon.property('profession', 'Theoretical Physicist')
```

### (C) Create a `livesWith` Edge from Leonard to Sheldon.

```
leonard.addEdge('livesWith', sheldon)
```

## Kata \#3 - Movie Recommendations

Movie Recommendations Graph TraversalSource

Get the [1m movie DB](http://grouplens.org/datasets/movielens/)

The katas in this section are an update to the commands listed at [A Graph-Based Movie Recommender Engine](http://markorodriguez.com/2011/09/22/a-graph-based-movie-recommender-engine/)

There are a number of setup steps before getting to the katas. You can copy/paste each block of code to speed through the setup steps.

Create a new graph:

```
graph = TinkerGraph.open()
g = graph.traversal()
```

Set a map to the representation of the user's occupation as string:

```
occupations = [0:'other', 1:'academic/educator', 2:'artist',
  3:'clerical/admin', 4:'college/grad student', 5:'customer service',
  6:'doctor/health care', 7:'executive/managerial', 8:'farmer',
  9:'homemaker', 10:'K-12 student', 11:'lawyer', 12:'programmer',
  13:'retired', 14:'sales/marketing', 15:'scientist', 16:'self-employed',
  17:'technician/engineer', 18:'tradesman/craftsman', 19:'unemployed', 20:'writer'] 
```

Add indexes to make the search for users and movies faster when adding the ratings later.

```
graph.createIndex('movieId', Vertex.class)
graph.createIndex('userId', Vertex.class)
```

Load the movie data:

```
new File('/opt/titan/movies-data/movies.dat').eachLine {def line ->
  def components = line.split('::');
  def movieVertex = graph.addVertex(label, 'movie', 'movieId', components[0].toInteger(), 'title', components[1]);
  components[2].split('\\|').each { def genera ->
    def hits = g.V().has('genera', genera);
    def generaVertex = hits.hasNext() ? hits.next() : graph.addVertex(label, 'genera', 'genera', genera);
    movieVertex.addEdge('hasGenera', generaVertex);
  }
}
```

Load the user data:

```
new File('/opt/titan/movies-data/users.dat').eachLine {def line ->
  def components = line.split('::');
  def userVertex = graph.addVertex(label, 'user', 'userId', components[0].toInteger(), 'gender', components[1], 'age', components[2].toInteger());
  def occupation = occupations[components[3].toInteger()];
  def hits = g.V().has('occupation', occupation);
  def occupationVertex = hits.hasNext() ? hits.next() : graph.addVertex(label, 'occupation', 'occupation', occupation);
  userVertex.addEdge('hasOccupation', occupationVertex);
}
```

Load the ratings data:

```
new File('/opt/titan/movies-data/ratings.dat').eachLine {def line ->
  def components = line.split('::');
  def user = g.V().has('userId', components[0].toInteger()).next();
  def movie = g.V().has('movieId', components[1].toInteger()).next();
  def ratedEdge = user.addEdge('rated', movie);
  ratedEdge.property('stars', components[2].toInteger());
}
```

Check the data loaded:

```
g.V().count()
g.E().count()
g.V().has(label, 'movie').count()
g.V().has(label, 'genera').count()
g.V().has(label, 'user').count()
g.V().has(label, 'occupation').count()
```

Results:

```
gremlin> g.V().count()
==>9962
gremlin> g.E().count()
==>1012657
gremlin> g.V().has(label, 'movie').count()
==>3883
gremlin> g.V().has(label, 'genera').count()
==>18
gremlin> g.V().has(label, 'user').count()
==>6040
gremlin> g.V().has(label, 'occupation').count()
==>21
```

### List the indexes on the vertices

```
graph.getIndexedKeys(Vertex.class)
```

There should be 2: movieId, userId

### Determine the distribution of occupations. You will need to use the `user` vertices, the `hasOccupation` edges and the `occupation` key(/value).

```
g.V().has(label, 'user').out('hasOccupation').values('occupation').groupCount()
```

### Determine the average age of all users.

```
g.V().has(label, 'user').values('age').mean()
```

### Assign the movie vertex for the `title` of `Toy Story (1995)` to the variable `v`.

```
v = g.V().has('title', 'Toy Story (1995)').next()
```

### Determine how many users gave Toy Story more than 3/4 stars.

```
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.count()
```

Notes:
* Answer >3: 1,655
* Answer >4: 820

### Show the `valueMap` for the first 5 users that rated Toy Story more than 3 stars.

```
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.outV()
  .range(0,5).valueMap()
```

### Determine which users gave Toy Story more than 3 stars and what other movies did they give more than 3 stars to.

```
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.outV()
  .outE('rated').filter{it.get().property('stars').value > 3}.inV().range(0,5)
  .values('title')
```

Notes
* Start from Toy Story - `g.V(v)`
* Get the incoming rated edges - `inE('rated')`
* Filter out those edges whose star property is less than 4 - `filter{it.get().property('stars').value > 3}`
* Get the tail user vertices of the remaining edges - `outV()`
* Get the rating edges of those user vertices - `outE('rated')`
* Filter out those edges whose star property is less than 4 - `filter{it.get().property('stars').value > 3}`
* Get the head movie vertices of the remaining edges - inV()
* Only emit the first 5 titles - `range(0,5)`
* Get the string title property of those movie vertices - `values('title')`

### TBD ~~Define a [user defined step](https://github.com/tinkerpop/gremlin/wiki/User-Defined-Steps) that takes star as a parameter and outputs the titles of the first 5 movies as per the previous kata.~~

Does not work:

```
Gremlin.defineStep('corated',[Vertex,Pipe], { def stars ->
_().inE('rated').filter{it.get().property('stars') > stars}.outV()
  .outE('rated').filter{it.get().property('stars') > stars}.inV()})
```

### Determine the number of results of the previous query's and how many of those results are unique. (2 queries)

```
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.outV()
  .outE('rated').filter{it.get().property('stars').value > 3}.inV()
  .count()
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.outV()
  .outE('rated').filter{it.get().property('stars').value > 3}.inV()
  .dedup().count()
```

Results:
==>268493
==>3353

### TBD: ~~Determine which titles have the most hits in the above query. (i.e. no dedup)~~

Does not work:

```
g.V(v).inE('rated').filter{it.get().property('stars').value > 3}.outV()
  .outE('rated').filter{it.get().property('stars').value > 3}.inV()
  .values('title').groupCount().range(0,5)
```

The above katas are incomplete. Need help with syntax.

---

## Kata \#4 - Northwind SQL 2 Gremlin

Source: [sql2gremlin.com](http://sql2gremlin.com/)

Setup:
* [Download Northwind DB](http://sql2gremlin.com/assets/northwind.groovy).
* Start Gremlin with Northwind: `bin/gremlin.sh /tmp/northwind.groovy`
* `graph = NorthwindFactory.createGraph()`
* `g = graph.traversal()`

For the katas just use the [sql2gremlin.com](http://sql2gremlin.com/) site and scroll to each T-SQL statement.
