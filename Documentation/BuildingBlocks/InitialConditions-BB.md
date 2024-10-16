## Initial Conditions (IC)
The Initial Conditions (IC) BB defines the containers in which the molecules will be present and their initial amounts. 

- When a simulation is created from modules, a "Molecule A" will only be created in a "Container 1" if there is at least one IC BB with the entry for "Molecule A" in "Container 1".


- The entries in an IC BB are not restricted to the molecules and/or containers in the module where the IC BB is located. An IC BB can overwrite or extend the initial conditions for all combinations of molecules and containers in the whole project.

- Each module can contain none or multiple IC BBs. During simulation creation, only one IC BB from each module can be selected.

### Extend
- To add new entries to the IC BB, use the "Exdend" button located in the "Edit Initial Conditions" ribbon bar. A dialog window opens allowing to select the molecules for which the entries should be created (molecules from different modules can be selected) and a spatial structure. Entries will be created for each selected molecule in each physical container of the selected spatial structure. To add entries for the containers in different modules, the extension step must be repeated for each module.
- Entries from different IC BBs can be combined to one IC BB by exporting an IC BB to pkml (right click -> "Save As PKML") and loading into existing IC BB (right click -> "Extend from IC BB").

- IC BB can be exported to Excel. Only values that are defined by a constant value are exported.

### Adding expression profiles to an IC BB

To add expression of a protein to a new structure, simply "extend" the IC BB and select the protein. Then add the expression parameters to the PV BB (see description of PB BB).