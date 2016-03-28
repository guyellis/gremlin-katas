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

## Kata \#1

### Load the Sugar plugin.

```
SugarLoader.load()
```

### Set the variable `graph` to an instance of the `Modern` graph and a `TraversalSource` instance of that graph to the variable `g`.

```
graph = TinkerFactory.createModern()
g = graph.traversal(standard())
```

### List the edges and vertices in the graph.

```
g.E()
g.V()
```

### Print the number of edges and vertices in this graph.

```
g.E().count()
g.V().count()
```

### Find the first vertex.

```
g.V(1)
or
g.V().next()
```

### Find the first 2 vertices.

```
g.V().range(0, 2)
```

### List all the keys and values for the vertices and edges.

```
g.V().valueMap()
g.E().valueMap()
```

### Find the vertex with the name 'marko'.

```
g.V().has('name', 'marko')
```

### Print the values that this vertex has.

```
g.V().has('name', 'marko').values()
```

### List the out-going edges of this vertex.

```
g.V().has('name', 'marko').outE()
```

### List the out-going edges of people that marko knows.

```
g.V().has('name', 'marko').outE('knows')
```

### List the vertices on the other side of those edges.

```
g.V().has('name', 'marko').outE('knows').inV()
```

### List the names associated with the vertices on the other side of those edges.

```
g.V().has('name', 'marko').outE('knows').inV().values('name')
```

### Shorten the `outE('knows').inV()` part of the previous statement to a single command.

```
g.V().has('name', 'marko').out('knows').values('name')
```

### List all the out/in/both edges with a 'knows' label.

```
g.V().outE('knows')
g.V().inE('knows')
g.V().bothE('knows')
```

### Assign the result from `g.V().has('name', 'marko')` to a variable named marko.

```
marko = g.V().has('name', 'marko').next()
```

Why `next()`? `V()` returns an iterator that will return the first element when `next()` is called.

### Find the `id/label` associated with the vertex (variable) `marko`.

```
g.V(marko).id()
g.V(marko).label()
```

### Find who Marko knows that has an age of 32 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', 32)
```

### Find who Marko knows that has an age greater than 30 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', gt(32))
```

### Show the names of who Marko knows that has an age greater than 30 using the just assigned variable `marko`.

```
g.V(marko).out('knows').has('age', gt(30)).values('name')
```

### Delete the vertex “marko” using the `marko` variable.

```
g.V(marko).drop()
```

### Confirm that the count of vertices is one less than it was before.

```
g.V().count()
```

### Remove the rest of the vertices.

```
g.V().drop()
```

### Confirm that the count of vertices is now zero.

```
g.V().count()
```

Info: All the edges will have been removed as well.

---

## Kata \#2

CRUD operations specified in parens before each kata.

### Set variable `graph` and `g` to a new empty graph and `TraversalSource`?

```
graph = TinkerGraph.open()
g = graph.traversal(standard())
```

### (C) Add a new vertex with label of 'person', a name of 'sheldon' and an age of 33?

```
g.addV(label, 'person', 'name', 'sheldon', 'age', 33)
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

### (CR) Add an edge between `sheldon` and `amy` with a label of dating. (Assign vertices to the variables `sheldon` and `amy`. Assign the edge to the variable `relationship`.)

```
sheldon = g.V().has('name', 'sheldon').next()
amy = g.V().has('name', 'amy').next()
relationship = sheldon.addEdge('dating', amy)
```

### (C) Add a `person` vertex with the name `leonard`, an age of `32` and a profession of `Experimental Physicist`. Assign this to variable `leonard`.

```
leonard = g.addV(label, 'person', 'name', 'leonard', 'age', 32, 'profession', 'Experimental Physicist').next()
```

### (U) Update Sheldon to be a `Theoretical Physicist`

```
sheldon.property('profession', 'Theoretical Physicist')
```

### (C) Create a `livesWith` Edge from Leonard to Sheldon.

```
leonard.addEdge('livesWith', sheldon)
```
