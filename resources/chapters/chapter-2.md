# Data patterns

In the previous chapter we looked at **data patterns**, i.e., vectors
after the `:where` clause of the form `[?e :movie/title "Commando"]`. 
There can be many data patterns in a `:where` clause:

    [:find ?title
     :where
     [?e :movie/year 1987]
     [?e :movie/title ?title]]

The important thing to note here is that the pattern variable `?e` is
used in both data patterns. When a pattern variable is used in
multiple places the query engine requires it to be bound to the same
value in each place. Therefor, the above query will only find movies
made in 1987.

The order of the data patterns does not matter (except possibly for
performance) so the previous query could just as well have been written

    [:find ?title
     :where
     [?e :movie/title ?title]
     [?e :movie/year 1987]]

and the result set would have been exactly the same.

Let's say we want to find out who starred in "Lethal Weapon". We
will need three data patterns for this. The first one finds the
entity-id of the movie with "Lethal Weapon" as the title:

    [?m :movie/title "Lethal Weapon"]

Using the same entity id, we can find the cast members with the data
pattern

    [?m :movie/cast ?p] 

`?p` will now be (the entity id of) a person entity so we can grab the
actual name with

    [?p :person/name ?name] 

The query will therefor be

    [:find ?name
     :where
     [?m :movie/title "Lethal Weapon"]
     [?m :movie/cast ?p]
     [?p :person/name ?name]]