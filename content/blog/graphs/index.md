---
title: Graphs
date: "2021-12-21T13:50:32.169Z"
description: Graph algorithms by Alvin Zablan
---

This is the summary of graph algorithms course by Alvin Zablan @ [Structy](https://www.structy.net/). 

I really like his teaching technique. He takes the time to explain things in detail. You can give it a free try at Structy too.


This blog is mostly a blue-print / pseudocode to myself. Not exhaustive or aims to teach the reader about the details, but rather a note to myself.

The graph algorithms begin with a search. Depth First versus Breath First.

Note that these are Directed Acyclical Graphs (DAGs). More on that later...

### Depth First Search

There are two ways to achieve the Depth First Search (DFS).

First pattern is recursive (obviously) and second pattern is a stack based loop. But either way, the graph is represented with an object/hashmap/dict, where key represents the vertex and values are the edges to the vertices. An example is:

```javascript
const graph = {
    a: ['b','c'],
    b: ['c']
    c: []
}
```

The recursive pattern pseudocode

``` javascript
const depth_first = (graph, vertex)  =>{
    // base case, execute and stop
    // recursive case 
    
    console.log(vertex) //
    for (let neighbour of graph[vertex]){
        depth_first(graph, neighbour)
    }
}
```

Loop pattern
```javascript
cosnt depth_first = (graph, starting_vertex)  => {
    // base case 
    // create a stack: const stack = [starting_vertex];
    // as long as there are elements on the vertex, process them 
    // then add each neighbour of the current vertex into the stack

    // i.e.
    const stack = [starting_vertex];
    while (stack.length > 0) {
        const current_vertex = stack.pop();
        for (let neighbour of graph[current_vertex]) {
            stack.push(neighbour);
        }
    }
}
```

### Breadth First Search 

For Breadth First Search (BFS), instead of implementing a stack, where the last one added is processed first, this will be a queue, where first in is first out. And add the children of the current item on the queue, to the end of the line. 

The pattern is a single loop over a queue. There is not a recursive pattern here.

Using the same graph object:

```javascript
const graph = {
    a: ['b','c'],
    b: ['c']
    c: []
}
```

BFS pseudo code for printing elements are:

``` javascript
const bread_first_search = (graph, vertex) => {

    // create a queue with the first item being the starting vertex
    // then as long as there are items to process in the queue
    // process it, and then add the children of the queue to the END of the queue 
    const queue = [vertex]
    while (queue.length > 0) {
        const current_vertex = queue.shift() // returns the first element from the queue, while removing it from the array;
        console.log(current_vertex)
        for (let neighbour of graph[current_vertex]) {
            queue.push(neighbour)
        }
    }
}
```

### Undirected Graphs

Undirected graphs are same as DAGs but edges are both ways. Assume that there is a vertex m and vertex n, and these are connected it would be represented as 

```javascript 
// eges are represented as an array of arrays, because there can be other vertices that are connected in the list
const edges = [['m','n']] 
```

Since we have the graph object as adjacency list, shown in the previous section, why not convert to it? 

```javascript
//convert edges to adjacency list, while keeping both directions
const graph = {
    m : ['n'],
    n : ['m']
}
```

But this bring cyclicality issues

```javascript
// edges can be cyclical too
// i points to j, which points to k but k can point back to i
const edges = [
    ['i', 'j'],
    ['j', 'k'],
    ['k', 'i'],
]
```

To avoid infinity loop above, the solution is to create a Set that tracks the vertices that we already visited.



