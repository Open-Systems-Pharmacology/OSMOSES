A concept of the implementation of the re-qualification of PB-QSP MoBi-projects utilizing the modularization concept.

The goal of the re-qualification framework is to compare simulation results produced with models build on different versions of PK-Sim platforms.

The structure of a PB-QSP MoBi-project that can be re-quialified is described in [WP2: Modularization concept](./WP2-Modularization-concept). In brief, the project consists of:

- A set of **PK-Sim modules**, being a PBPK model imported from PK-Sim.
- **Individuals**, being sets of parameter values describing the physiology of an individual.
- **Protein Expression** profiles, being a set of parameters describing the expression (and localization) of proteins in an individual.
- **Extension modules**, consisting of Building Blocks (BB).
- **Model configurations**, being a combination of modules.
  * Model configurations may contain **initial conditions (IC)** that are not present in the original modules. Upon model configuration creation, these entries are populated according to default rules. The user can change these values (manually or importing from CSV/previously created BBs). The IC special for the model configuration can be stored (e.g. as a PKML) and be imported as new modules, or imported into an existing model configuration.
- **Models** as instances of model configurations. User can change values of the parameters in the models.

During the re-qualification process, the project will be rebuilt from the **MoBi-snapshot**.

## MoBi-snapshot
For each MoBi-project, a **snapshot** can be created from the File-menu. The structure of the MoBi-snapshot is:

- List of PK-Sim modules within the project. Each entry consists of the unique **name** of the module.
- List of PK-Sim snapshots. Each PK-Sim module in a MoBi-project has an assigned PK-Sim snapshot with all the information required to re-create the whole PK-Sim model (i.e., the combination of basic PBPK and PK-Sim Extension modules). Each PK-Sim sub-snapshot has a unique ID. Every PK-Sim snapshot is exported to MoBi-snapshot only **once**, with it's ID stored in the list.
- List assigning snapshot ID to PK-Sim module names
- List of individual BBs. Each entry contains the information necessary to re-create the individual parameter set and the name of the individual.
- List of expression profiles. Every entry contains the whole expression profile.
- List of Extension modules, containing the name of the module. Additionally, the module will be exported as PKML.
- Observed data (as in PK-Sim snapshot)
- List of model configurations, containing the name, ordered list of modules (names), name of the individual (if applicable), name of the expression profile (if applicable), and the list of user-changed initial conditions _IC_diff_ and parameter values _PV_diff_.
- List of models, including the name and ID of the model, name of the model configuration the model is created from, name of the selected individual, name of selected expression profile, simulation settings such as output intervals, solver settings, user-defined parameter values _P_Vdiff_ and initial conditions _IC_diff_, and the mapping of model outputs with observed data (== plot configuration).

- ## MoBi Snapshot
  - ### PK-Sim modules
    - Name
  - ### PK-Sim snapshots
    - ID
    - Snapshot information required to re-create the PK-Sim model
  - ### Snapshot <-> Model map
    - PK-Sim snapshot ID
    - PK-Sim module name
  - ### Individuals
    - Species
    - Population
    - Gender
    - Age
    - Weight
    - Height
    - User-defined Anatomy and Physiology
    - Name
  - ### Extension modules
    - Name
    - Exported PKML
  - ### Observed data
    - See PK-Sim snapshot
    - Name
    - xVals
    - yVals
    - Metadata
  - ### Model configurations
    - Name
    - Ordered list of Module names
    - Name of Individual
    - Name of Expression Profile
    - User-changed Initial conditions _IC_diff_
    - User-changed Parameter values _PV_diff_
  - ### Models
    - Name
    - Name of Model configuration
    - Name of Individual
    - Name of Expression Profile
    - User-changed Initial conditions _IC_diff_
    - User-changed Parameter values _PV_diff_
    - Simulation settings (output intervals, solver settings...)
    - Plot configuration
      - Mapping of model output <-> observed data
      - Curve colors etc...

## Steps of MoBi project re-qualification:

1. Create the **snapshot** of the project.
    1. For each model configuration _MC_ of the project:
        1. Internally, re-create a copy of the model configuration by combinding the PK-Sim modules with the Extension modules as defined in model configuration. This will create a model configuration _MC'_ with IC and PV populated based on default rules.
    1. Compare IC of _MC_ and _MC'_ and store the differences _IC_diff_.
    1. Compare PV of _MC_ and _MC'_ and store the differences _PV_diff_.
    1. For each model within the model configuration, compare the parameter values and initial conditions with values in _MC'_. Store differences _PV_diff_model_ and _IC_diff_model_.
1. Load snapshot of the PK-Sim module into PK-Sim and create a model from the snapshot.
1. Send the new model (incl. new snapshot) to MoBi.
1. If a read-only Individual with the same name exists - overwrite it with the new created individual. If a non-read-only individual with the same exists, import the new individual with a new unique name.
1. Load Extension modules from pkml.
1. Recreate model configurations.
1. IC that are missing in the original modules are population according to default rules. Apply _IC_diff_ and _PV_diff_.
1. Re-create models and apply _PV_diff_model_ and _IC_diff_model_.
1. Run simulations and compare results (workflow from here on identical to PK-Sim re-qualification).
