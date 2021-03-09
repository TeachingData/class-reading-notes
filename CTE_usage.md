# Common Table Expressions

Common Table Expressions (CTEs) can be thought of as saved queries within the scope of a single SQL statement. They consist of two types:
- Ordinary (*we will use in class*)
- Recursive (*we will discuss during analytics*)

Both use the "*WITH*" statement in front of an INSERT, DELETE, UPDATE, or SELECT statement.

## Ordinary common table expressions 
These can be used to improve readability by factoring out subqueries from a SQL statement. They are the main ones you will use in class.
#### Syntax/Example: 
```
WITH 'cte_name' (return_field <, optional_ones>) AS (... some query ...) SELECT return_field from cte_name;

-- can have multiple ctes using WITH

WITH 'people_attended' (num_attended) AS (SELECT count() FROM attended WHERE attended=1),
     'total_people' (num_people) AS (SELECT count() FROM members)
     SELECT ROUND((select num_attended from people_attended) * 1.0 /
                  (select num_people from total_people), 2) AS percent_attended
     FROM attended;     
```

A WITH clause defaults to an ordinary CTE even if one uses the RECURSIVE flag. 
RECURSIVE does not force CTE's to be recursive only makes it possible.

## Recursive Common Table Expressions
Recursive CTE are used for hierarchical and/or recursive queries of trees and graphs. These are sometimes used during the ETL process to help with analysis in larger NoSQL systems

#### Syntax
Its syntax is the same as a standard CTE but with a few additional attributes:

1. Select statment (as of 3.34 or 12/2020) must be a compound one involving UNION, INTERSECT, or EXCEPT
2. Must contain at least 1 recursive Select
    - A Select is recursive if it's FROM clause references the CTE name
3. All non-recursive statements must happen first (before recusive one)
    - These should connect to the recursive statement using UNION or INTERSECT
4. Recursive statements cannot contain aggregated functions (non-recursive are fine)

```
WITH RECURSIVE cte-table-name AS (
  ... first select statement ...
  UNION
  ... recursive select statement...
)
```

What this does is:

1. Run the initial select adding the results to a queue.
2. Until the queue is empty
    - Extract a row from queue (one at a time).
    - Optimizer determines row is only used once so *returns then discards row as only result*
    - Runs the recursive select and adds its results to the queue (similar return & discard each time).

#### Recursive Query Example For Loop
The following query emulates the old "blastoff for loop countdown" that most do in their intro classes:

```
-- blastoff countdown from 10 to 0
WITH RECURSIVE
  blastoff(countdown) AS (SELECT 10 UNION ALL SELECT countdown-1 FROM blastoff WHERE countdown>0)
  -- each time this is called it UNIONs on 10 and "last value" - 1 so only returns the new value
  -- once "last value" - 1 is 1-1 it returns nothing because 0 is not greater than 0 and recursion exits
SELECT countdown FROM blastoff;
```

This query:
1. First select runs and returns the single column *"10"* which it adds to the queue. 
2. This row and column (*"10"*) is then extracted, added to *"blastoff"*, and discarded from the queue
3. Then the recursive select recalls blastoff and (seeing that 9 is greater than 0) adds the new value to the queue
4. Step 2-3 repeat until 0 is reached and no values are added to the queue (cause 0 = 0 not greater) causing it to be empty.

#### Recursive Query Example Graphing
Again this is mostly used when progressing through graph data (edges) so a good working example is:
**WE WILL NOT BE PERFORMING GRAPHING USING THIS METHOD BUT ITS GOOD TO KNOW ITS POSSIBLE**

Suppose you have an undirected graph where each node is identified by an integer and edges are defined by the following table. 
If we want to see all the nodes connected to ***"THE ANSWER TO LIFE THE UNIVERSE AND EVERYTHING"*** we'd query with:

```
CREATE TABLE edges(x INT, y INT);

WITH RECURSIVE nodes(a) AS (
   SELECT 42
   -- base case (first select) of 42
   UNION
   SELECT x FROM edges JOIN nodes ON y=a
   -- follow (traverse) edges from y to x (and add these to our results)
   UNION
   SELECT y FROM edges JOIN nodes ON x=a
   -- traverse edges from x to y (and add new ones, UNION not UNION ALL, to our results)
)
SELECT a FROM nodes; -- start the recursion
```
