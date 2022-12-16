## Expression Profile in MoBi
- An expression profile is a list of parameter paths and values that define the expression of a protein (relative expression AND intitial concentration and all associated parameters)
- Values can be edited by user, but no new entries can be added or existing entries removed
- (TO BE IMPLEMENTED) Ontogeny/variability part of the BB?
    - Variability cannot be stored in MoBi
- How to present the BB? List of parameter paths/values (like the PSV-BB) plus some meta information:
    - Name of the protein
    - Type (enzyme/binding partner/transporter)
    - (TO BE IMPLEMENTED) PK-Sim version
- "Export to pkml": stores a pkml with all required information.
- Load from pkml as a ExpProfile-BB

- (TO BE IMPLEMENTED) Load from pkml as PV-BB in xModule - load only parameter paths - values pairs
    - Alternative - "Send as PV-BB to Module..."

### Export from PK-Sim
- (TO BE IMPLEMENTED) One BB per Expression profile in PK-Sim (only for the model that is exported, not ALL profiles within the PK-Sim project)

### Create a new Profile

- Access to PK-Sim ExpProfile creation dialogue
- (TO BE IMPLEMENTED) Access Exp DB

### Use-cases and solution
- Add a new compartment to the SS
    - Expression parameter must be part of the PV-BB of the extension module
- Add new protein to the project
    - Create ExpProfile, also from DB?
   
### Tasks
- Export from PK-Sim creates ExpProfile-BBs

### Notes