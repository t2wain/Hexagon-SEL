# Understanding Hexagon Smart Electrical (SEL) Data Dictionary (DD)

The Data Dictionary defines the following:
- Physical model of the database such as tables and columns. These tables are setup in other database schemas separate from the data dictionary schema. 
- Logical model of objects such as Motor and Cable
- Mapping between the physical and logical models on how data are stored
- Relationship between the objects of the logical model

There are 3 different Data Dictionaries in a SEL application:
- Site Data Dictionary
- Plant Data Dictionary
- SEL Data Dictionary

## DD tables that define the physical model

- Entities -> Database table
    - Entity_Number -> ID
    - Entity_Apps -> database schemas
- Attributes -> Database column
    - Attribute_Number -> ID
- UniqueAtts -> column in table
    - Entity_Number -> table
    - Attribute_Number -> column
- Relationships -> 2 related CorrelAtts in an SQL table join
    - Rel_Number
    - Rel_Type -> SUBCLASS, ASSOC_NC, GROUP
    - Source_Entity -> table
    - Source_Coratt -> CorrelAtt
    - Source_Schema - database schemas
    - Dest_Entity -> table 
    - Dest_Coratt -> CorrelAtt
    - Dest_Schema -> database schemas
- CorrelAtts -> columns in an SQL table join
    - CorrelAtt_Number
    - Number_Of_Atts
    - Attributes -> columns

## DD tables that define the logical model

- Item
    - ID
    - Name
    - SourceTable -> Entity
    - AppSchemas -> database schema
 - ItemAttributions
    - ItemID -> Item
    - Name
    - DisplayName
    - AttributionID -> UniqueAtts
    - Path -> sequence of Relationship
- SourceDestObjectRels
    - ParentItemID -> Item
    - ChildItemID -> Item
    - Path -> sequence of Relationship

## The logical model

*Item* table defines the logical objects. Each logical object is associated with a main database table (*SourceTable*). Examples of SEL logical object are *Motor*, *Cable*, and *Bus*.

*ItemAttributions* table defines the properties (*AttributionID*) of the logical objects. Object's properties are stored across multiple database tables. The properties are grouped together by a common *Path* object which specifies the table that stores the properties. A *Path* is a sequence of *Relationship* objects where each *Relationship* defines an SQL join clause between two database tables. This sequence of table joins between the main table and the related tables is used in an SQL query to retrieve the object's properties.

## Motor example

Motor is a logical object (*Item*) and it has 789 properties (*ItemAttributions*) which are stored across 61 tables grouped by *Path*. The database table, T_Motor, is the main table for the Motor object. An SQL query can be generated for each *Path* to retrieve the object's properties from various database tables.

<pre>
Item : Motor (159), Entity : T_Motor (135)
</pre>

<pre>
Path : 410_377_531_634
select
   T_RefElectricalEquipment.RatedVoltage RefCircuit_RatedVoltage, -- T(C154), F(Text), V(T)
   T_RefElectricalEquipment.Frequency RefCircuit_Frequency -- T(C114), F(Text), V(T)
from T_Motor@spel
   inner join T_Load@spel on (T_Load.SP_ID = T_Motor.SP_ID) -- RN(410), RT(SUBCLASS), DC(1), SC(1)
   inner join T_ElectricalEquipment@spel on (T_ElectricalEquipment.SP_ID = T_Load.SP_ID) -- RN(377), RT(SUBCLASS), DC(1), SC(1)
   inner join T_RefCircuit@spelref on (T_RefCircuit.SP_ID = T_ElectricalEquipment.SP_RefCircuitID) -- RN(531), RT(ASSOC_NC), SC(1), DC(88)
   inner join T_RefElectricalEquipment@spelref on (T_RefElectricalEquipment.SP_ID = T_RefCircuit.SP_ID) -- RN(634), RT(SUBCLASS), DC(1), SC(1)
</pre>

<pre>
Path : 410_377_576_733_25_42
select
   T_ModelItem.SP_ID AlternatePowerSourceEquipment_SP_ID, -- T(S32), F(Text), V(F)
   T_ModelItem.ModelItemType AlternatePowerSourceEquipment_ModelItemType, -- T(C40), F(Text), V(I)
   T_ModelItem.Description AlternatePowerSourceEquipment_Description, -- T(S4000), F(Text), V(T)
   T_ModelItem.ItemTypeName AlternatePowerSourceEquipment_ItemTypeName -- T(S40), F(Text), V(I)
from T_Motor@spel
   inner join T_Load@spel on (T_Load.SP_ID = T_Motor.SP_ID) -- RN(410), RT(SUBCLASS), DC(1), SC(1)
   inner join T_ElectricalEquipment@spel on (T_ElectricalEquipment.SP_ID = T_Load.SP_ID) -- RN(377), RT(SUBCLASS), DC(1), SC(1)
   inner join T_Equipment@spel on (T_Equipment.SP_ID = T_ElectricalEquipment.SP_ID) -- RN(576), RT(SUBCLASS), DC(1), SC(1)
   inner join T_Equipment@spel e5 on (e5.SP_ID = T_Equipment.SP_AlternatePowerSourceID) -- RN(733), RT(ASSOC_NC), SC(1), DC(159)
   inner join T_PlantItem@spel on (T_PlantItem.SP_ID = e5.SP_ID) -- RN(25), RT(SUBCLASS), DC(1), SC(1)
   inner join T_ModelItem@spel on (T_ModelItem.SP_ID = T_PlantItem.SP_ID) -- RN(42), RT(SUBCLASS), DC(1), SC(1)
</pre>

## Comments included in the generated SQL

The SQL queries are generated based on the information from the Data Dictionary. In the generated SQL, comments are included that reference such information from the Data Dictionary used to generate that part of the SQL query.

Comments in the header:
- Item (logical object) name and ID
- Entity (database table) name and ID
- Path for this particular group of properties

Comments in the SQL query:
- Information about each property
    - Logical datatype - T(C40)
    - Formatted display - F(Text)
    - Visibility - V(T)
- Information about each table join
    - Relationship ID - RN(410)
    - Relationship type - RT(SUBCLASS)
    - Destination CorrelAtt ID - DC(1)
    - Source CorrelAtt ID - SC(1)
    - Table joins are generated in the same sequence as listed in the Path

The generated SQL query with comments is a valid Oracle statement and can be executed as-is.

## Hexagon SEL2018 ERD Diagram

Hexagon documentation includes an ERD diagram that also include the following reference information from the Data Dicationary
- Relationship ID (in red)
- Item ID (in pink)
- Entity ID (in pink)

Therefore, the comments in the generated SQL query is helpful to matchup with the information in the ERD diagram.

## Property relationships

Some properties (*ItemAttributions*) defined for an object are about itself while other properties are about its other related objects. There are several type of relationships between objects:
- Subclass
- Association
- Group

*Subclass* describes the relationship of objects from general to specific type. An example is:
- Equipment -> Electrical Equipment -> Load -> Motor 

*Association* describes the dependency between objects. Examples are:
- Equipment -> Equipment (alternate power source)
- Equipment -> Reference Circuit

Group describes the whole/part relationship between objects. Examples are:
- PDB Section -> Cell
- Cable -> Conductor

Each *Relationship* object in the *Path* has a type of relationship defined.

## Engineering Data Editor in SEL

The Engineering Data Editor feature in SEL allows user to query the object's properties and display the data in a grid windows. The available properties are those defined in the *ItemAttributions* table.

## Parent / child relationship between objects

*SourceDestObjectRels* defines the parent and child relationship between objects. Each parent/child relationship has a *Path* that defines a sequence of table joins. Examples for Cable are:

<pre>
Path : 412_419_575_654 -  ( Cable.PowerSource.Equipment )
select e5.* 
from T_Cable@spel
   inner join T_ElectricalConductor@spel on (T_ElectricalConductor.SP_ID = T_Cable.SP_ID) -- 412, SUBCLASS
   inner join T_WiringEquipment@spel on (T_WiringEquipment.SP_ID = T_ElectricalConductor.SP_ID) -- 419, SUBCLASS
   inner join T_Equipment@spel on (T_Equipment.SP_ID = T_WiringEquipment.SP_ID) -- 575, SUBCLASS
   inner join T_Equipment@spel e5 on (e5.SP_ID = T_Equipment.SP_PowerSourceID) -- 654, ASSOC_NC
</pre>

<pre>
Path : 730 -  ( RefGland.GlandSide1.Cable )
select T_Cable.* 
from T_RefGland@spelref
   inner join T_Cable@spel on (T_Cable.SP_RefGlandSide1ID = T_RefGland.SP_ID) -- 730, ASSOC_NC
</pre>

## SEL Reporting

SEL allows user to create complex report that outputs data to an Excel file. The query in the report starts with a base object type, such as Cable, and can include other related object types as specified in the *SourceDestObjectRels* table.

## Reference documents

This repository include the following generated SQL files:
- Site ItemAttributions SQL.txt
- Plant ItemAttributions SQL.txt
- SEL ItemAttributions SQL.txt
- SEL SourceDestObjectRels SQL.txt

Other files are:
- SEL2018 ERD.pdf