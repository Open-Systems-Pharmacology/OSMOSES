## Initial Conditions (IC)
The Initial Conditions (IC) BB defines the containers in which the molecules will be present and their initial amounts.

By default, an IC BB does not contain any enries. When a simulation is created from modules, a "Molecule A" will only be created in a "Container B" if there is at least one IC BB with the entry for "Molecule A" in "Container B". To add new entries to the IC BB, use the "Exdend" button located in the "Edit Initial Conditions" ribbon bar. A dialog window opens allowing to select the molecules for which the entries should be created (molecules from different modules can be selected) and a spatial structure. Entries will be created for each selected molecule in each physical container of the selected spatial structure. To add entries for the containers in different modules, the extension step must be repeated for each module.

The entries in an IC BB are not restricted to the molecules and/or containers in the module where the IC BB is located. An IC BB can overwrite or extend the initial conditions for all combinations of molecules and containers in the whole project.

Each module can contain none or multiple IC BBs. During simulation creation, only one IC BB from each module can be selected.

## Initial Conditions (IC) Building Block (BB)
- An IC BB is an $n \times m$ matrix with $n$ the number of molecules and $m$ the number of containers, consisting of molecule names, container paths, start values, Scale Divisors, and the "is present" and "negative values allowed" properties
- Values can be edited by user, new entries can be added or existing entries removed
- "Export to pkml": stores a pkml with all required information.
- Load from pkml as a new IC-BB
- Merge from PKML into exisiting IC BB
- Export to excel
- Load from excel

![Modules with IC BBs as they would be created following current definition.](../Figures/IC_BB_Example_alt.png)

### Extend
- used for adding entries e.g. after adding new molecules
- shows a UI where user can select which entries to add
- shows all possible combinations of
  - molecules of the respective module and all containers across all modules. **ALTERNATIVE** - select a module (PK-Sim or xModule) and show combinations of molecules and containers of the two modules (the current module which IC will be extended and the selected module). Benefit of this solution - easy way to create IC entries for modules which only add containers but no molecules.
  - "Is present" property is pre-selected according to the default values for the containers

### Export from PK-Sim
- Does not include entries for proteins as there will be part of the "Experssion"-BBs.

### Use-cases and solutions
- Change IC values in and xModule
- Overwrite IC with formulas
- Different IC e.g. for different individuals or disease states. Example: a "Glucose" module. How to store different ICs?  We do not want to duplicate all the molecules and reactions etc. in a new module, only new ICs...
    - F2F 2021: Multiple IC and PV BBs per module
   
### Tasks
- Comparison
- Save to pkml
- Load from pkml

### Notes

