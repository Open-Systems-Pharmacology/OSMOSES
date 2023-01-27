A new BB-type **Individuals** will be introduced. This BB is not part of a module but will be shared within the project. The entries of this BB contain spatial structure parameter sets describing individual physiology.

In addition to parameter values, each Individual-entry stores the information needed to re-create the specific individual. The information is:
- Species
- Population
- Gender
- Age
- Weight
- Height
- User-defined Anatomy & Physiology (as in the corresponding PK-Sim tab).

## Individuals in MoBi
- An Individual is a list of parameter paths and values that define the physiology of an individual. First version - only constant paramters (no derived params)
- For future: also include formula parameters and re-think the presentation of this BB
- Values can be edited by user, but no new entries can be added or existing entries removed
- How to present the BB? List of parameter paths/values (like the PSV-BB) plus some meta information:
    - Like the "Biometrics"-tab in PK-Sim
- "Export to pkml": stores a pkml with all required information.
- Load from pkml as a Individual-BB
- Load from pkml as PV-BB in xModule - load only parameter paths - values pairs
    - Alternative - "Send as PV-BB to Module..."

### Export from PK-Sim
- Whole simulation: One individual per PK-Sim export (plus required expression profiles)
- Optional: export individual -> Loads the individual AND the expression profiles in MoBi

### Create a new Individual
- Access to PK-Sim UI
- To allow **aging**, this property must be selected upon creation of the individual in PK-Sim. In this case, the corresponding parameters well be represented by table formulas.

### Use-cases and solutions
- Add a new compartment to the SS
    - Expression parameter must be part of the PV-BB of the extension module
   
### Tasks
- "Individual"-class for MoBi?
- Individuals in the BuildingBlocks-explorer
- Export from PK-Sim creates one Individual-BB
- UI for showing the Individual
- Comparison of individuals
- Save to pkml
- Load from pkml
- "Send as PV-BB to Module..."

### Notes
- Move to core as late as possible
- Create mock-up for the look of the BB