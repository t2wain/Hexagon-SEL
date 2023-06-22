# Understanding Hexagon Smart Electrical (SEL) Data Dictionary (DD)

The Data Dictionary defines the following:
- Physical model of the database such as tables and columns. These tables are setup in other database schemas separate from the data dictionary schema. 
- Logical model of objects such as Motor and Cable
- Mapping between the physical and logical models on how data are stored
- Relationship between the objects of the logical model

The benefits of understanding the Data Dictionary are:
- Knowing properties for items helps you plan and create Engineering Data Editor (EDE) in SEL
- Understand the relationships between items helps you write SEL reports
- Help you perform the property mappings of Data and Association links in the Import Manager
- Help you write SQL statements to export the data to other applications

There are 3 different Data Dictionaries in a SEL application:
- Site Data Dictionary
- Plant Data Dictionary
- SEL Data Dictionary

## DD tables that define the physical model

- Entities -> describe database table
    - Entity_Number -> ID
    - Entity_Apps -> ref. database schemas
- Attributes -> describe database column
    - Attribute_Number -> ID
- UniqueAtts -> describe column in table
    - Entity_Number -> ref. table
    - Attribute_Number -> ref. column
- Relationships -> describe 2 database columns in an SQL table join
    - Rel_Number
    - Rel_Type -> SUBCLASS, ASSOC_NC, GROUP
    - Source_Entity -> ref. table
    - Source_Coratt -> ref. CorrelAtt
    - Source_Schema - ref. database schemas
    - Dest_Entity -> ref. table 
    - Dest_Coratt -> ref. CorrelAtt
    - Dest_Schema -> ref. database schemas
- CorrelAtts -> describe columns in an SQL table join
    - CorrelAtt_Number
    - Number_Of_Atts
    - Attributes -> ref. database columns

## DD tables that define the logical model

- Item -> describe logical object
    - ID
    - Name
    - SourceTable -> ref. Entity
    - AppSchemas -> ref. database schema
 - ItemAttributions -> describe item properties
    - ItemID -> ref. Item
    - Name
    - DisplayName
    - AttributionID -> ref. UniqueAtts
    - Path -> ref. sequence of Relationship
- SourceDestObjectRels -> describe more relationships of objects
    - ParentItemID -> ref. Item
    - ChildItemID -> ref. Item
    - Path -> ref. sequence of Relationship

## The logical model

*Item* table defines the logical objects. Each logical object is associated with a main database table (*SourceTable*). Examples of SEL logical object are *Motor*, *Cable*, and *Bus*.

*ItemAttributions* table defines the properties (*AttributionID*) of the logical objects. Object's properties are stored across multiple database tables. The properties are grouped together by a common *Path* object which specifies the table that stores the properties. A *Path* is a sequence of *Relationship* objects where each *Relationship* defines an SQL join clause between two database tables. This sequence of table joins between the main table and the related tables is used in an SQL query to retrieve the object's properties.

The Data Dictionary also contains the definitions of all the custom attributes created for the items using the Data Dictionary Manager.

## Motor example

Motor is a logical object (*Item*) and it has 789 properties (*ItemAttributions*) which are stored across 61 tables grouped by *Path*. The database table, T_Motor, is the main table for the Motor object. An SQL query can be generated for each *Path* to retrieve the object's properties from various database tables.

<pre>
Item : Motor (159), Entity : T_Motor (135)

Path : 410_377_531_634
select
   T_RefElectricalEquipment.RatedVoltage RefCircuit_RatedVoltage, -- T(C154), F(Text), V(T)
   T_RefElectricalEquipment.Frequency RefCircuit_Frequency -- T(C114), F(Text), V(T)
from T_Motor@spel
   inner join T_Load@spel on (T_Load.SP_ID = T_Motor.SP_ID) -- RN(410), RT(SUBCLASS), DC(1), SC(1)
   inner join T_ElectricalEquipment@spel on (T_ElectricalEquipment.SP_ID = T_Load.SP_ID) -- RN(377), RT(SUBCLASS), DC(1), SC(1)
   inner join T_RefCircuit@spelref on (T_RefCircuit.SP_ID = T_ElectricalEquipment.SP_RefCircuitID) -- RN(531), RT(ASSOC_NC), SC(1), DC(88)
   inner join T_RefElectricalEquipment@spelref on (T_RefElectricalEquipment.SP_ID = T_RefCircuit.SP_ID) -- RN(634), RT(SUBCLASS), DC(1), SC(1)

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

## Item relationships

Some properties (*ItemAttributions*) defined for an item are about itself while other properties are about its other related items. There are several type of relationships between items:
- Subclass
- Association
- Group

*Subclass* describes the relationship of items from general to specific types. In general, all attributes created for a generic item will also be available to the more specific item. Therefore, when creating a custom attributes for an item, you should consider the *Subclass* relationships of the items. An example is:
- Equipment -> Electrical Equipment -> Load -> Motor 

*Association* describes the dependency between items. Examples are:
- Equipment -> Equipment (alternate power source)
- Equipment -> Reference Circuit

Group describes the whole/part relationship between items. Examples are:
- Power Cables -> Parallel grouping
- Single-Core Power Cables -> Single-Core Cable Assembly grouping

Each *Relationship* object in the *Path* has a type of relationship defined.

## Hexagon SEL2018 ERD Diagram

Hexagon documentation includes an ERD (Entity Relationship Diagram) diagram that also include the following reference information from the Data Dicationary
- Relationship ID (in red)
- Item ID (in pink)
- Entity ID (in pink)

Therefore, the comments in the generated SQL query will help to correlate the information in the ERD diagram.

## Engineering Data Editor in SEL

The Engineering Data Editor feature in SEL allows user to query the object's properties and display the data in a grid windows. The available properties are those defined in the *ItemAttributions* table.

## Parent / child relationship between objects

*SourceDestObjectRels* defines the parent and child relationship between objects. Each parent/child relationship has a *Path* that defines a sequence of table joins. Examples for Cable are:

<pre>
Item : Cable

Path : 412_419_575_654 -  ( Cable.PowerSource.Equipment )
select e5.* 
from T_Cable@spel
   inner join T_ElectricalConductor@spel on (T_ElectricalConductor.SP_ID = T_Cable.SP_ID) -- RN(412), RT(SUBCLASS), DC(1), SC(1)
   inner join T_WiringEquipment@spel on (T_WiringEquipment.SP_ID = T_ElectricalConductor.SP_ID) -- RN(419), RT(SUBCLASS), DC(1), SC(1)
   inner join T_Equipment@spel on (T_Equipment.SP_ID = T_WiringEquipment.SP_ID) -- RN(575), RT(SUBCLASS), DC(1), SC(1)
   inner join T_Equipment@spel e5 on (e5.SP_ID = T_Equipment.SP_PowerSourceID) -- RN(654), RT(ASSOC_NC), SC(1), DC(139)


Path : 730 -  ( RefGland.GlandSide1.Cable )
select T_Cable.* 
from T_RefGland@spelref
   inner join T_Cable@spel on (T_Cable.SP_RefGlandSide1ID = T_RefGland.SP_ID) -- RN(730), RT(ASSOC_NC), DC(157), SC(1)
</pre>

## SEL Reporting

SEL allows user to create complex report that outputs data to an Excel file. The query in the report starts with a base item type, such as Cable. From the base item, you can build a hierachy of related items (Define -> New) to retrieve more data. The relationship options for each item in the hierachy are specified in the *SourceDestObjectRels* table.

The relationships in the *SourceDestObjectRels* table are quite abstract. You will need an understanding of the Data Dictionary, a familiarity with the functionalities in SEL, and some experimentation in the SEL report to fully understand the meaning of these relationships. One learning strategy is to systematically correlate a functionality in SEL that generates data for such relationship in the database. Or, view the data of such relationship in the database (with the generated SQL) and correlate it to a functionality in SEL (reverse-engineering).

## Reference documents

This repository include the following files.

Items listing:
- Site Logical Items.txt
- Plant Logical Items.txt
- SEL Logical Items.txt

ItemAttributions listing:
- Site ItemAttributions.txt
- Plant ItemAttributions.txt
- SEL ItemAttributions.txt

Generated ItemAttributions SQL:
- Site ItemAttributions SQL.txt
- Plant ItemAttributions SQL.txt
- SEL ItemAttributions SQL.txt

Object relations:
- SEL SourceDestObjectRels.txt
- SEL SourceDestObjectRels SQL.txt

Other files are:
- SEL2018 ERD.pdf