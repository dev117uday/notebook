# Graph DB : Neo4j

- Entity : noun : are nodes
- Relationships : verb : vertices
- Properties : to describe both noun and relationship
    - Adjectives : about noun : about nodes
    - Adverbs : about verb : about relationship
- Variables are temporary : just there to store value

### Creating Nodes

```
# to find all nodes and return them
match(n) return n

# create node with variable
create (n:Person)
return n

create (n:Employee)
return n

create (anne :Person:Employee:Footballer)
return anne

# create node with variable n and property [name, age, email]
create (n{name:"Peter", age:25, <email:"p@gmail.com>"})
return n

# create node with variable n and label Person and property [name, age, email]
create (n:Person{name:"Anne", age:20})
return n

create (n:Dancer{name:"Li", age:22})
return n

# multiple labels
create (n:Dancer:Supplier{name:"Li", age:22})
return n

# creating relationships
create (n:Supplier{name:'A'}), (m:Client{name:'B'}),
(n)-[:Supplied]->(m)
return n, m

create (n:Supplier{name:'A'})-[r:Supplied]-> (m:Client{name:'B'})
return m,n,r

create (n:Supplier{name:'A'})-[r:Supplied]-> (:Client{name:'B'})
return *

# string named relationship
create (a{name:'Andrew'})<-[:`the forgotten friend of`]-(m{name:'Mike'})
return *

create (a:Person{name:'Andrew'})<-[:`the forgotten friend of`]-(m:Person{name:'Mike'})
return *

create
(d:Person{name:"Dan"}), (c:Vehicle{name:"Car"}), (a:Person{name:"Andrew"}), (e:Person{name:"Eric"}),
(dog:Animal{name:"Dog"}), (l:Location{name:"AB Street"}), (crime:Crime{title:"Killing of a dog"}),
(d)-[:Party_to]->(crime)<-[:Involved_in]-(c), (a)-[:Party_to]->(crime)<-[:Investigated_by]-(e),
(dog)-[:Victim_of]->(crime)<-[:Occurred_at]-(l), (dog)-[:Killed_at]->(l)
return *

create
(D:Person{name:'Dan'}), (K:Person{name:'Kate'}), (M:Person{name:'Mike'}), (L:Person{name:'Luke'}),
(S:Person{name:'Steve'}), (F:Person{name:'Favour'}), (faith:Person{name:'Faith'}), (J:Person{name:'Jane'}),
(D)-[:MARRIED_TO]->(K)-[:MARRIED]->(D), (D)-[:PARENT_OF]->(M)<-[:PARENT_OF]-(K),
(D)-[:PARENT_OF]->(L)<-[:PARENT_OF]-(K), (D)-[:PARENT_OF]->(S)<-[:PARENT_OF]-(K),
(F)-[:MARRIED_TO]->(S)-[:MARRIED]->(F), (F)-[:PARENT_OF]->(faith)<-[:PARENT_OF]-(S),
(F)-[:PARENT_OF]->(J)<-[:PARENT_OF]-(S)
RETURN *

# create node from another node
create (a{name:'A', city:'New York'})

match(a{name:'A'})
create(n{name:'B', city:a.city})
return n.city

# copy relationship property
create
(a{name:'A'}), (b{name:'B'}), (c{name:'C'}), (a)-[r:KNOWS{since:'2016'}]->(b)
return *

match(a{name:'A'})-[r]->(b{name:'B'})
create (b)<-[r2:KNOWS{since:r.since}]-(c{name:'c'})
return properties(r2)

```

### Matching Nodes

```
# to match all nodes
match(n) return n

# match labels
match(n:Person) return n
match(n:Property) return n

# to find all nodes with label
match(p{name:'Steve'})--(n) return *

# match with nodes of another label
match(n{name:"Robert Zemeckis"})--(m:Movie)
return *

# match with nodes with incoming relation
match(polar{title:"The Polar Express"})<--(k)
return *

```

### Return Clause

```
# to return name property of all nodes
match (n) return n.name

# to return multiple properties
match (n) return n.name, n.born

# just like SQL " select name, 'Actors' ", appends new column to each row
match (n) return n.name, 'Actors'

# simple math operations, as Calculator means the result title will be 'Calculator'
return 6*6 as Calculator

# to return all relationship btw nodes
match(n)-[r]-(m)
return r

# to return type of relationship btw node
match(n)-[r]-(m)
return type(r)

# to find relation btw 2 nodes
match(n{name:"Frank Darabont"})-[r]->(m{title:"The Green Mile"})
return r

# to return relationship properties
match(n)-[r]-(m)
return r.roles

# to find specific relationship
match(n)-[r:DIRECTED]-(m)
return r

# to find type of specific relationship
match(n)-[r:DIRECTED]-(m)
return type(r)

# to return with alias
match(n) return n.name AS NAME, n.born

# to find nodes
match(n)
return distinct n.born
order by n.born ASC

# to return properties
match (n)
return properties(n)

# for specific proporties
match (n{name:"Tom Hanks"})
return properties(n)

# return properties of relationship
match (n{name:"Tom Hanks"})-[r]->()
return properties(r)

match()-[r]->()
return properties(r)

match()-[r]->()
return *

# return all labels
create(n:Person:Actor:Director:Producer{name:'Victor'})

match(n{name:"Victor"})
return labels(n)

# return distant relationship
match p = ({name:"Paul Blythe"})-->()-->()-->()
return p

match p = ({name:"Paul Blythe"})-->()-->()-->({title:"Cloud Atlas"})
return relationships(p)

```