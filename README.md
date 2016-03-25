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

### How do you load the Sugar plugin?

```
SugarLoader.load()
```

### How do you set the variable `graph` to an instance of the `Modern` graph and a `TraversalSource` instance of that graph to the variable `g`?

```
graph = TinkerFactory.createModern()
g = graph.traversal(standard())
```

### How do you list the edges and vertices in the graph?

```
g.E()
g.V()
```

### How many edges and vertices are there in this graph?

```
g.E().count()
g.V().count()
```

### How do you find the first vertex?

```
g.V(1)
```

### How do you find the first 2 vertices?

```
g.V().range(0, 2)
```

### How do you find the vertex with the name 'marko'?

```
g.V().has('name', 'marko')
```
### How do you list all the keys and values for the vertices and edges?

```
g.V().valueMap()
g.E().valueMap()
```

### What values does this vertex have?

```
g.V().has('name', 'marko').values()
```

### How do you list the out-going edges of this vertex?

```
g.V().has('name', 'marko').outE()
```

### How do you list the out-going edges of this vertex of people that marko knows?

```
g.V().has('name', 'marko').outE('knows')
```

### How do you list the vertices on the other side of those edges?

```
g.V().has('name', 'marko').outE('knows').inV()
```

### How do you list the names associated with the vertices on the other side of those edges?

```
g.V().has('name', 'marko').outE('knows').inV().values('name')
```

### How do you shorten the outE('knows').inV() to a single command?

```
g.V().has('name', 'marko').out('knows').values('name')
```

### How do you list all the out/in/both edges with a 'knows' label?

```
g.V().outE('knows')
g.V().inE('knows')
g.V().bothE('knows')
```

### How do you assign the result from `g.V().has('name', 'marko')` to a variable named marko?

```
marko = g.V().has('name', 'marko').next()
```

Why `next()`? `V()` returns an iterator that will return the first element when `next()` is called.

### How do you find the id/label associated with the vertex (variable) marko?

```
g.V(marko).id()
g.V(marko).label()
```

### How do you find who Marko knows that has an age of 32 using the just assigned variable marko?

```
g.V(marko).out('knows').has('age', 32)
```

### How do you find who Marko knows that has an age greater than 30 using the just assigned variable marko?

```
g.V(marko).out('knows').has('age', gt(32))
```

### How do you show the names of who Marko knows that has an age greater than 30 using the just assigned variable marko?

```
g.V(marko).out('knows').has('age', gt(30)).values('name')
```

### How do you delete the vertex “marko” using the marko variable?

```
marko.drop()
```

### How do you confirm that the count of vertices is one less than it was before?

```
g.V().count()
```

### How do you remove the rest of the vertices?

```
g.V().drop()
```

### How do you confirm that the count of vertices is now zero?

```
g.V().count()
```

Info: All the edges will have been removed as well.

### How do you add a new vertex with label of 'person', a name of 'sheldon' and an age of 33?

```
g.addV(label, 'person', 'name', 'sheldon', 'age', 33)
```

### How do you find the id of the vertex that was just added?

```
g.V().id()
Or
g.V().next().id()
```

### How do you add a new vertex with label of 'person', a name of 'amy' and an age of 31?

```
g.addV(label, 'person', 'name', 'amy', 'age', 31)
```

### How do you add an edge between `sheldon` and `amy` with a label of dating? (Assign vertices to the variables `sheldon` and `amy`. Assign the edge to the variable `relationship`.)

```
sheldon = g.V().has('name', 'sheldon').next()
amy = g.V().has('name', 'amy').next()
relationship = sheldon.addEdge('dating', amy)
```
