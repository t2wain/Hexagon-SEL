# Tips for Hexagon Smart Electrical (SEL)

## Import electrical loads

A typical start of a SEL project is to import a large set of data including

- Electrical loads (Motor and OEE)
- PBDs and buses that supply these loads 

As a prerequisite, the following configurations should be completed first:

- Naming Conventions are setup for these items:
    - Bus (based on PDB)
    - Circuit (based on connected equipment)
    - Cable (based on connected equipment)
    - Control station (based on associated equipment)
- Equipment Profiles are setup for these items:
    - Motor (w/ power cable and control station)
    - Control station (w / control cable)

Next, import the following items using Import Manager 

- Loads (along with reference to bus on a separate property)

Create PDB and bus using Reference PDB item (with bus), then update the PDB tag

- PDB
- Bus

Next perform the following actions in SPEL

- Batch load association (w/ create circuit)
- Apply motor profile, which will create
    - Power cable connected between circuit and motor
    - Control station associated with the motor, which will create
        - Control cable connected to the control station
- Connect control cable to the circuit

Alternatively, all items and associations can be imported using the Import Manager.

## Tips on the setup of Naming Convention

This file provides more information on some logic related to naming convention:

- NamingConvention.xml 

## Reference Electrical Engineer

Using the Reference Electrical Engineer to duplicate a design of an existing load (motor) feeder for a new load.

- Open the Reference Electrical Engineer for the same plant
- Create a new circuit in the bus of destination plant
- Drag the power cable from reference plant to the circuit in the destination plant, which will create
    - Power cable, connected to circuit and motor
    - Associated control station of the motor
    - Control cable associated with the control station
- Update the motor tag

## Replication with Reference Electrical Engineer

Reference Electrical Engineer can also be use to replicate data between units with similar design in the same plant. A poosible strategy might be as follow:

- First, backup and restore to create a copy of the original plant

Next, prepare the tags for the new items

- In the copy plant, copy the original tag and ID to separate properties (ex. Name) for future reference
- Export those original tag that were maintained manually, then update them to the new tag for the new unit
- Import the updated tags back into another separate property for future reference

To perform the replication

- Login to the new unit in the original plant
- Open the Reference Electrical Engineer and connect to the copy plant
- Copy over the electrical system to the original plant
- Using Import Manager to copy over the new tags that are stored in the separate property
- If naming conventions were setup, the item tags of related items will also be updated

The advantages of using Reference Electrical Engineer to copy the data is that

- All properties (which can be numerous) will be copied
- All related items will be copied
- All association relationships will be copied

## Replication with Import Manager

As an alternative, you would have to use the Import Manager to replicate the plant unit:

- Export all items and all their properties
- Export all the connectivity and relationships
- Update the item tags
- Setup the Import Manager project to map all the items and their properties
- Execute the import links in sequence to replicate the data

I think such effort is quite daunting.