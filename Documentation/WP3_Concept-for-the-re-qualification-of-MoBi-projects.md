A concept of the implementation of the re-qualification of PB-QSP MoBi-projects utilizing the modularization concept.

The goal of the re-qualification framework is to compare simulation results produced with models build on different versions of PK-Sim platforms.

The structure of a PB-QSP MoBi-project that can be re-qualified is described in [WP2: Modularization concept](./WP2-Modularization-concept). In brief, the project consists of:

- A set of **PK-Sim modules**, being a PBPK model imported from PK-Sim.
- **Individuals**, being sets of parameter values describing the physiology of an individual.
- **Protein Expression** profiles, being a set of parameters describing the expression (and localization) of proteins in an individual.
- **Extension modules**, consisting of Building Blocks (BB).
- **Simulation configurations**, being a combination of a modules, 0 or 1 individual, and 0-many expression profiles.
- **Models** as instances of simulation configurations. User can change values of the parameters in the models.

During the re-qualification process, the project will be rebuilt from the **MoBi-snapshot**.

## MoBi-snapshot
For each MoBi-project, a **snapshot** can be created from the File-menu. The structure of the MoBi-snapshot is:

- List of PK-Sim modules within the project. Each entry is a PK-Sim snapshot of the PK-Sim building blocks and PK-Sim simulation required to recreate the module in PK-Sim.
- List of Extension modules. Each module will be exported as PKML into the snapshot.
- List of individual BBs.
  - For individual building blocks created as part of a PK-Sim module import, the export contains the PK-Sim snapshot that can be used to recreate the individual in PK-Sim.
  - For individual building blocks created in MoBi, the export will be PKML.
- List of expression profiles.
  - For expression profile building blocks created as part of a PK-Sim module import, the export contains the PK-Sim snapshot that can be used to recreate the expression in PK-Sim.
  - For expression profile building blocks created in MoBi, the export will be PKML
- Observed data (as in PK-Sim snapshot)
- Simulations containing many of the same members as PK-Sim (charts, mappings, output selections), but also containing
  - parameter values that have changed since creation
  - Simulation configuration with
    - Simulation settings
    - Optional name of individual building block
    - Optional list of names of expression profiles
    - Ordered list of module configurations (at least one)
      - Optional Parameter Values to use 
      - Optional Initial Conditions to use.

- ## MoBi Snapshot
  - ### PK-Sim modules
    - Snapshot information required to re-create the PK-Sim module
  - ### Extension modules
    - Exported PKML
  - ### Individuals - from PK-Sim module import
    - Snapshot information required to re-create the PK-Sim Individual
    - Altered values
  - ### Individuals - created in MoBi
    - Exported PKML
  - ### Expression Profile - from PK-Sim module import
    - Snapshot information required to re-create the PK-Sim Expression Profile
    - Altered values
  - ### Expression Profile - created in MoBi
    - Exported PKML
  - ### Observed data
    - See PK-Sim snapshot
  - ### Simulations
    - See PK-Sim snapshot, includes name, output mappings, chart specifications
    - Configuration with
      - settings (see PK-Sim snapshot)
      - optional individual building block
      - optional list of expression building blocks 
      - ordered list of module configurations (at least one)
        - module name
        - optional name of parameter values
        - optional name of initial conditions

## Steps of MoBi project re-qualification:

1. Load snapshots of the PK-Sim modules into PK-Sim and create models from the snapshots.
2. Send the new models (incl. new snapshot) to MoBi.
3. Load snapshots of individuals and expression profiles into PK-Sim and create the building blocks.
5. Send the new individuals and expressions (incl. new snapshot) to MoBi.
6. Apply altered values to individual and expression profile building blocks
7. Load Extension modules from pkml.
8. Load PKML individuals and expression profiles.
9. Recreate module configurations.
10. Rebuild the model and export as PKML
11. Reporting workflow follows PK-Sim workflow from here.
