## Parameter Values (PB) Building Block (BB)
- A PV BB is a list of parameter paths/names and values
- For the "state variable" parameters, start values are listed

- Values can be edited by user, new entries can be added or existing entries removed
- New entries can be added with a dialogue which allows to select from a tree or type in the path.

- PV BB can be exported to Excel. Only values that are defined by a constant value are exported.
- Entries from different PV BBs can be combined to one PV BB by exporting a PV BB to pkml (right click -> "Save As PKML") and loading into existing PV BB (right click -> "Extend from PV BB").
- Parameters that are defined in the PV BB but are not present in any other BB (e.g., not defined in the Spatial Structure) will be added upon simulation creation.

### Add local molecule parameters
For selected molecule(s), adds entries for parameters that are defined as _local_ to all selected containers, if the container criteria. Only _constant_ local parameters are added. If a parameter is defined by a formula, it is assumed that it should not be overwritten by a PV BB. _Note_: the parameter can still be added manually.

### Adding expression profiles to a PV BB
- Values from an expression profile can be loaded to a PV BB by exporting an expression profile to PKML and loading into the PV BB.
- Alternatively, use the "Add Protein Expression" button located in the "Edit" tab.