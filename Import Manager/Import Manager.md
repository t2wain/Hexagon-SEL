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

```xml
<Link Name="Motor" Type="Data" >
    <Source>Motor</Source>
    <Target>Motor</Target>
    <Query>
        <SQL Source="BRAKEPOWER" Target="BrakePower" Key="N"/>
        <SQL Source="DESCRIPTION" Target="Description" Key="N"/>
        <SQL Source="EFFICIENCY75LOAD" Target="Efficiency75Load" Key="N"/>
        <SQL Source="EFFICIENCYFULLLOAD" Target="EfficiencyFullLoad" Key="N"/>
        <SQL Source="EFFICIENCYHALFLOAD" Target="EfficiencyHalfLoad" Key="N"/>
        <SQL Source="EFFICIENCYOPERATING" Target="EfficiencyOperating" Key="N"/>
        <SQL Source="FLACALCULATIONFLAG" Target="FLACalculationFlag" Key="N"/>
        <SQL Source="FREQUENCY" Target="Frequency" Key="N"/>
        <SQL Source="ISLOAD" Target="IsLoad" Key="N"/>
        <SQL Source="ISSLD" Target="IsSLD" Key="N"/>
        <SQL Source="ITEMTAG" Target="ItemTag" Key="Y"/>
        <SQL Source="ITEMTAGFULLNAME" Target="ItemTagFullName" Key="N"/>
        <SQL Source="LRCTOFLARATIO" Target="LRCToFLARatio" Key="N"/>
        <SQL Source="MOTORRATEDPOWER" Target="MotorRatedPower" Key="N"/>
        <SQL Source="OPERATINGFACTOR" Target="OperatingFactor" Key="N"/>
        <SQL Source="OPERATINGMODE" Target="OperatingMode" Key="N"/>
        <SQL Source="POWERFACTOR75LOAD" Target="PowerFactor75Load" Key="N"/>
        <SQL Source="POWERFACTORATSTARTING" Target="PowerFactorAtStarting" Key="N"/>
        <SQL Source="POWERFACTORFULLLOAD" Target="PowerFactorFullLoad" Key="N"/>
        <SQL Source="POWERFACTORHALFLOAD" Target="PowerFactorHalfLoad" Key="N"/>
        <SQL Source="POWERFACTOROPERATING" Target="PowerFactorOperating" Key="N"/>
        <SQL Source="RATEDVOLTAGE" Target="RatedVoltage" Key="N"/>
        <SQL Source="STARTINGCURRENT" Target="StartingCurrent" Key="N"/>
    </Query>
</Link>
```

<pre>
Data Name: Motor
Source: Motor
Target: Motor
   ITEMTAG => ItemTag (K)
   BRAKEPOWER => BrakePower
   DESCRIPTION => Description
   EFFICIENCY75LOAD => Efficiency75Load
   EFFICIENCYFULLLOAD => EfficiencyFullLoad
   EFFICIENCYHALFLOAD => EfficiencyHalfLoad
   EFFICIENCYOPERATING => EfficiencyOperating
   FLACALCULATIONFLAG => FLACalculationFlag
   FREQUENCY => Frequency
   ISLOAD => IsLoad
   ISSLD => IsSLD
   ITEMTAGFULLNAME => ItemTagFullName
   LRCTOFLARATIO => LRCToFLARatio
   MOTORRATEDPOWER => MotorRatedPower
   OPERATINGFACTOR => OperatingFactor
   OPERATINGMODE => OperatingMode
   POWERFACTOR75LOAD => PowerFactor75Load
   POWERFACTORATSTARTING => PowerFactorAtStarting
   POWERFACTORFULLLOAD => PowerFactorFullLoad
   POWERFACTORHALFLOAD => PowerFactorHalfLoad
   POWERFACTOROPERATING => PowerFactorOperating
   RATEDVOLTAGE => RatedVoltage
   STARTINGCURRENT => StartingCurrent
</pre>

## Association (Relation) links

Among the import link types (Data, Association, Delete, SelectList, etc.), the Association link type is the most ambigous. Which item types can be associated with one another, which item type should be the parent, which item type should the child, and what is the result of the association are not always intuitive. Perhaps, these documents in the SEL application folder can provide some hints:

- RelationTemplate.xml
- SpelImportManagerItemTypes.xml

## ItemHierachy vs. Plant Unit

ItemHierachy is an item property that refers to the plant unit of which the item belongs to. It seems this property exists only in the Import Manager application. You can map to this property inside the Data link type and the Association link type. 

In the Association link, if the parent/child items belong to different plant units, ItemHierachy property must be mapped for both items.

The typical data format of ItemHierachy property is "\area\unit". The plant hierachy data is stored in the database table/column T_PLANTGROUP.DIR_PATH of the Plant schema.