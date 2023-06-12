# Tips for Hexagon Smart Electrical (SEL) Import Manager

## Import Manager project demos

The SEL installation folder has the following sample import project files:

- Demo.xml
- Ref_Cable_ImportLink.xml
- ReferenceCableWayComponents.xml
- SPI_Import.xml

The import project file is an XML document and can be view in a text editor application. Alternatively, you can view the generated outlines of the project files as below:

- Demo.xml.txt
- Ref_Cable_ImportLink.xml.txt
- ReferenceCableWayComponents.xml.txt
- SPI_Import.xml.txt

## Association (Relation) links

Among the import link types (Data, Association, Delete, SelectList, etc.), the Association link type is the most ambigous. Which item types can be associated with one another, which item type should be the parent, which item type should the child, and what is the result of the association are not always intuitive. Perhaps, these documents in the SEL installation folder can provide some hints:

- RelationTemplate.xml
- SpelImportManagerItemTypes.xml