## Minimal Viable Product

### Use-case - adding a tumor as an extension module

### Package 1:

- Access PK-Sim from MoBi
- Expression Profile BB
- Individual BB
- Add a new layer of MoBi project organization - **Modules**
    - UI
        - Groups for:
            - PK-Sim modules
            - Expression Modules
            - Extra: Individuals and ExpProfiles
    - Logic

- ---
- A Module contains the following BBs:
    - Spatial Structure (SS)
        - "PARENT" property of a container
        - Save to pkml (resp. as a module) neighborhoods leading to containers *outside* of the available structure (e.g., for the tumor use case, neighborhoods to "Organism|VenousBlood|Plasma")
    - Molecules
    - Reactions
    - Passive Transports (PT)
    - Initial Conditision (IC)
    - Parameter Values (PV)

- Differentiate between **PK-Sim** modules
    - Save snapshot
    - Not editable
- and **xModules**
    - Editable
    - PK-Sim module can be converted to xModule

- First xModule will only contain Reactions-BB with one reaction
- Creating **Model configuration** from modules
- (Saving/loading a module as PKML)
- Combine one PK-Sim module with one xModule containing one additional reaction