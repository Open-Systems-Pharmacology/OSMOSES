## Initial Conditions (IC) Building Block (BB)
- An IC BB is a list of molecule names, container paths, and start values...
- Formulas
- Values can be edited by user, new entries can be added or existing entries removed
- "Export to pkml": stores a pkml with all required information.
- Load from pkml as a new PV-BB
- Merge from PKML into exisiting PV BB
- Export to excel
- Load from excel

An n x m matrix, with n the number of molecules and m the number of containers, with start values of molecules, scaling factors, and boolean properties for "isPresent" and "Negative values allowed".
 post Workshop:
- “m” is only the number of containers/compartments, where molecules are actually present
- if an entry for molecule `a` in container `b` is not in the list, the molecule is "not present" in this container - is this still valid??

„extend“ functionality:
- used for adding entries e.g. after adding new molecules
- shows a UI where user can select which entries to add
- shows all possible combinations of
  - molecules of the respective module and all containers across all modules
  - pre-selected according to the default values for the containers

### Extend

### Export from PK-Sim
- Empty?

### Use-cases and solutions
- Change IC values in and xModule
- Overwrite IC with formulas
   
### Tasks
- Comparison
- Save to pkml
- Load from pkml

### Notes
