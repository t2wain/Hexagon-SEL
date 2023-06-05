# Understanding Hexagon Smart Electrical (SEL) Data Dictionary (DD)

The Data Dictionary defines the following:
- Physical model of the database such as tables and columns. These tables are setup in other database schemas separate from the data dictionary schema. 
- Logical model of objects within the application such as Motor and Cable
- Mapping between the physical and logical models on how data are stored
- Relationship between the objects of the logical model

## DD tables that define the physical model

- Entities -> Database table
    - AppName -> database schema
- Attributes -> Database column
- UniqueAttrs -> a set of columns in a table
    - EntityNumber -> Table
    - AttributeNumber -> Column
- Relationships -> 2 related CorrelAttr in an SQL table join
- CorrelAttr -> a table/column in an SQL table join

## DD tables that define the logical model

- Item
    - EntityNumber -> Entity
- Attributions
    - ItemID -> Item
    - Path -> sequence of Relationship

## The logical model

*Item* table defines the logical objects. Each logical object is associated with a main database table (*Entity*). Examples of SEL logical object are *Motor*, *Cable*, and *Bus*.

*Attributions* table defines the properties of the logical objects. Object's properties are stored in its main table and across other related database tables. The properties are grouped together by a common *Path* object which specifies the table that stores the properties. A *Path* is a sequence of *Relationship* objects where each defines an SQL join clause between two database tables. This sequence of table joins between the main table and the related tables is used in an SQL query to retrieve the object's properties.

## Motor example

As an example, Motor is a logical object (*Item*) that has 100 properties (*Attributions*) which are stored across 8 tables (*Entities*). The database table, T_Motor, is the main table for the Motor object.