# DataMeta

# Introduction to DataMeta

DataMeta is a cross-platform highly flexible
and highly expandable metaprogramming toolset aimed specifically at:

* greatly reducing code entropy, in many cases to a very substantial degree.
* shift runtime complexity into compile-time complexity, maximizing runtime performance.

Consider this typical use case: 

* What you have:

    * Definition of some data structure(s) (a.k.a. metadata).
    * Definition of data formats when applicable, such as for reading/writing
      structured [PTFs](https://en.wikipedia.org/wiki/Plain_text).

* What you need to have:
    * Several applications written in different languages for different
      platforms and frameworks (Java, Scala, Python, Ruby, SQL, noSQL, 
      [newSQL](https://en.wikipedia.org/wiki/NewSQL), Avro, AbInitio etc etc
      etc...) using data with the given structure(s).

To accomplish this task, DataMeta features:

* A collection of [DSLs](https://en.wikipedia.org/wiki/Domain-specific_language)
    designed to describe an aspect most clearly,
    most efficiently. The following languages are currently brought into the picture:
    * **DataMetaDOM** - DataMeta <u>D</u>omain <u>O</u>bject <u>M</u>odel, used to 
      describe data structures (metadata). Here's one [example of DataMetaDOM
code](FIXME) 
      that demonstrates its self-explanatory nature. The only thing that 
      needs to be explained is that the plus sign `+` as the first symbol
      of a field definition denotes a **required** field and the minus sign `-`
      denotes the optional field, the rest usually becomes instantly clear
      provided some familiarity with structured data in general.
      A showcase of all features of the DataMetaDOM 
      language can be found [&rarr;here](FIXME).
      DataMetaDOM is the most active and widely used DataMeta DSL.
    * Another one is **DataMetaForm** - format description language, usually used 
      in together with the relevant DataMetaDOM definition, for the cases when 
      format is applicable to a case of data
      handling, such as parsing/rendering of the PTFs.
    * There is also a plan to implement a language named **DataMetaFlow** to 
      describe a data flow in the same platform-independent fashion. 

* A set of importers/exporters from/to other metadata/structured data 
  description formats implemented on as-needed basis:
    * MySQL DDL - export and import, for a variety of tasks/projects.
    * Oracle SQL DDL, export only - originally for XStream/Golden Gate projects.
    * AbInitio, some formats, import only, originally for the ETL team.
    * Data Tools Web Table Structure, import only.
    * Avro schema - import/export, a bit outdated, before DataMetaDOM implemented
      aggregated field types such as `set`, `list`, `deque` and `map`.

* A collection of code generators that export for the variety of target
platforms:
    * Value objects with equality, hash function, getters/setters checking
      required fields for NULL/None/Nil values, data integrity verifiers,
      versioning. 
    * Static comparators to compare the value objects by either the record
      identity as defined by the DataMetaDOM `identity` statement or by the 
      full set of records.
    * Serializers/Deserializers to/from byte arrays.
    * Hadoop Writables, most efficient in terms of storage use and the 
      runtime.
    * This list can be also expanded with anything that becomes needed.

Implementing one importer/exporter takes from hours to a handful of days,
this list can be easily expanded/improved as needed.

Example:
* From [this DataMetaDOM definition](FIXME), were generated:
    *  [these Java classes](FIXME) that include value objects + static comparators + Hadoop writables + byte array
        serializers/deserializers,
    * and here the matching identical [artifacts for Python](FIXME).

Generated artifacts are made available to wider teams via package distribution
systems such as Maven, Ruby gems, Python eggs etc.

Imagine maintaining those by hand as the schema changes.

Minimizing the code entropy, having defined the:

* Data structure(s),
* Format(s) when/if applicable,
* Source/target platform specification,
      
The actual writen code is derivative, i.e redundant. DataMeta makes it happen
appear free of charge.

In an extreme case, from a single AbInitio file, a whole application had been
generated, complete with deployment code, to read compressed PTFs 
and load them in a SequenceFile into HDFS.

