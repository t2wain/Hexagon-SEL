# Tips for Hexagon Smart Electrical (SEL) Import Manager

## Import Manager project demos

The SEL application folder has the following sample import project files:

- Demo.xml
- Ref_Cable_ImportLink.xml
- ReferenceCableWayComponents.xml
- SPI_Import.xml

The import project file is an XML document and can be view in a text editor application. Alternatively, you can view the generated outlines of the project files listed below:

- Demo.xml.txt
- Ref_Cable_ImportLink.xml.txt
- ReferenceCableWayComponents.xml.txt
- SPI_Import.xml.txt

## Association (Relation) links

Among the import link types (Data, Association, Delete, SelectList, etc.), the Association link type is the most ambigous. Which item types can be associated with one another, which item type should be the parent, which item type should the child, and what is the result of the association are not always intuitive. Perhaps, these documents in the SEL application folder can provide some hints:

- RelationTemplate.xml
- SpelImportManagerItemTypes.xml

## ItemHierachy vs. Plant Unit

ItemHierachy is an item property that refers to the plant unit of which the item belongs to. It seems this property exists only in the Import Manager application. You can map to this property inside the Data link type and the Association link type. 

In the Association link, if the parent/child items belong to different plant units, ItemHierachy property must be mapped for both items.

The typical data format of ItemHierachy property is "\area\unit". The plant hierachy data is stored in the database table/column T_PLANTGROUP.DIR_PATH of the Plant schema.