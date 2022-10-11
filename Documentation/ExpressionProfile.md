## Expression Profile in MoBi
- An expression profile is a list of parameter paths and values that define the expression of a protein (relative expression AND intitial concentration values if changed in PK-Sim?)
- Contains snapshot that is sufficient to re-create the profile (by calling PK-Sim) - discarded, workflow would be to update the whole PK-Sim module
- Values can be edited by user, but no new entries can be added or existing entries removed
- Edit possible at the step of ExpProfile creation
- Ontogeny/variability part of the BB?
- How to present the BB? List of parameter paths/values (like the PSV-BB) plus some meta information:
    - Name of the protein
    - Type (enzyme/binding partner/transporter)
    - PK-Sim version
- "Export to pkml": stores a pkml with all required information.
- Load from pkml as a ExpProfile-BB
- Load from pkml as PV-BB in xModule - load only parameter paths - values pairs
    - Alternative - "Send as PV-BB to Module..."

### Export from PK-Sim
- One BB per Expression profile in PK-Sim (only for the model that is exported, not ALL profiles within the PK-Sim project)

### Create a new Profile

- Access to PK-Sim ExpProfile creation dialogue
- Access Exp DB

### Use-cases and solution
- Add a new compartment to the SS
    - Expression parameter must be part of the PV-BB of the extension module
- Add new protein to the project
    - Create ExpProfile, also from DB?
   
### Tasks
- "ExpressionProfile"-class for MoBi?
- ExpProfile in the BuildingBlocks-explorer
- Export from PK-Sim creates ExpProfile-BBs
- UI for showing the profile
- Comparison of profiles
- Save to pkml
- Load from pkml

### Notes
- Move to core as late as possible
- Create mock-up for the look of the BB