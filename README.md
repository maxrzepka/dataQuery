# dataQuery

A non-intrusive solution to query data of any kind.

  - Plain file in any format ( excel , pdf, xml ).
  - Relational DB
  - Other storage

The pipe processing is performed directly from the source.
Any conventional DataWarehouse solution first extracts all data in a big black box : it's called ETL
On contrary with this solution it

Here it's just optional for performance reason to perform join never a constraint.

The goal is to produc analytics starting with the aggregation operation likewise the SQL's group by.
Is there a need for a new query language ?
No, just simple datastructure à la datalog is enough like in cascalog or datomic.

Process data to produce analytics starting with the simple but quite useful SQL's group by .
So need of a new query language ? No just simple datastructure is enough à la datalog (cf cascalog and datomic).

## Concepts

### Schema

The source can be in various format but source need a unique schema for queries.
The definition of source can be split into 2 parts :
  - storage : how access the data for example file-system information (path , format) , db host and more specific storage infos
  - schema : inspired from Kimball's star-schema, it uses the notions of dimension and measure

```clojure
{:dimension [:type :region :country],
 :key [:acct],
 :measure [:balance :commission],
 :fields [:acct :type :region :country :balance :commission],
 :data
 ({:region "Europe",
   :type "B",
   :country "Germany",
   :acct "A1",
   :balance 203,
   :commission 8.97}
 ...)
 }
```

### Query

A query is a plain map which at the execution expandes to a cascalog query , SQL or other API.

#+begin_src clojure
{ :aggregate #{:type} :filter {:region "Europe"} :source s}
#+end_src

### Storage

As the schema and query language is independent of the storage,
adding new type of storage should be straightforward.

### Custom Columns
Adding new column should be also declarative.
Here an example of a column named balance-bins groups records according their balance buckets.

```clojure
{:balance-bins [:bucket :balance 0 1000 10000]}
```

