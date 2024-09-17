# OSMOSES modularization concept

The following document specifies the new concept of organizing modeling projects in modules to achieve better flexibility and re-usability in MoBi.

The new concept introduces changes on how model structures are organized and combined into simulations. While the current implementation of the software (OSPS V11.3) has two major layers of organization of a MoBi project – **Building Blocks** (BB) that are combined into **Simulations**, the new modularization concept extends the structure to **Modules**, **Building Blocks**, and **Simulations**.

## Definitions:
- **Entity**: Everything within the model structure - molecule, container, parameter, parameter value, etc.
- **Building Block (BB)**: Specific model parts that, combined together, create a full model structure. See documentation on [building blocks](https://docs.open-systems-pharmacology.org/working-with-mobi/mobi-documentation/building-block-concepts).
- **Module**: A set of BBs
  - _PK-Sim module_: A module based on a PK-Sim PBPK model. As best practice, a PK-Sim module should not be modified. Instead, all changes/extensions to the model should be done in the so-called Extension modules (see below). A PK-Sim module is converted into an Extension module when edited by the user.
  - _Extension module_: Editable modules that contain any changes to the model structure made by the user.

- **PK-Sim snapshot**: A part of the PK-Sim module allowing the re-creation of the PK-Sim module.
- **Model**: A module or a combination of modules.
- **Model structure**: Structural definition of the model, including containers, connections, species, active and passive processes, but excluding the parametrization and initial conditions of the final simulation.
- **Simulation**: Combination of the **model** and the results of a simulation of this model. *Note:* The distinction between a model and a simulation is not obvious in the OSPS, and these terms are interchangeable to a greater extent.
- **MoBi-project**: A MoBi-file (e.g. *.mbp3) containing modules, models, observed data, etc.
- [*Initial Conditions (IC) BB*](BuildingBlocks/InitialConditions-BB.md): A list of entries defining the start values of molecules in different compartments. Formerly known as Molecule Start Values.
- [*Parameter Values (PV) BB*](BuildingBlocks/ParameterValues-BB.md): A list of entries defining the values of the parameters (or the start values of state variable parameters). Formerly known as Parameter Start Values.
- **Individual**: Parameter set describing the physiological properties of an individual. The parameter set referred to is limited to the parameters provided by PK-Sim. Technically comparable to the Parameter Values BB with additional metadata. See [*Individuals*](BuildingBlocks/Individuals-BB.md) for details.
- **Expression profile**: Parameter set describing the expression of a protein. See [*Expression Profile*](BuildingBlocks/ExpressionProfile.md) for details.

## Project structure
A MoBi project contains a set of:
- PK-Sim modules
- Extension modules
- Individual BBs
- Expression Profile BBs
- Simulations, which are combinations of (0-n) PK-Sim modules, (0-n) Extension modules, (0-1) individual BB, and (0-n) expression profiles. At least one module (PK-Sim or Extension) must be selected to create a simulation.

## Modules
Every module consists of **building blocks (BB)**, with BB types [*Spatial Structures (SS)*](BuildingBlocks/SpatialStructures-BB.md) (0-1), *Molecules* (0-1), *Reactions* (0-1), [*Passive Transports (PT)*](BuildingBlocks/PassiveTransports-BB.md) (0-1), *Observers* (0-1), *Events* (0-1), [*Parameter Values (PV)*](BuildingBlocks/ParameterValues-BB.md) (1-n), and [*Initial conditions (IC)*](BuildingBlocks/InitialConditions-BB.md) (1-n). Every module includes no or exactly one BB of each type, except for PV and IC BBs, of which multiple can be present in one module.

### PK-Sim modules
A project in MoBi can be based on a PBPK model exported from PK-Sim. Such a model will be present as a **PK-Sim module** in MoBi containing *all of the BB types*. PK-Sim modules cannot be edited by default. If the user decides to edit a PK-Sim module, the PK-Sim module will be converted to an Extension module. A project can contain multiple or no PK-Sim modules.

Export of a PK-Sim model to MoBi creates one PK-Sim module, one individual, and (0-n) expression profile BBs.

Compared to the previous versions, v12 introduces some changes in how and where individual parameters are stored when a PBPK model is imported in MoBi. The Spatial Structure only contains parameters that are present in all species and have the same value or the same formula across all species and individuals. Furthermore, the PV BB only holds values for parameters that are different from those stored in other BBs. In most cases, the PV BB will be empty when a PK-Sim model is imported into MoBi. An exception is when the user has modified parameter values in the PK-Sim simulation that are not part of any PK-Sim BB (e.g., intestinal permeability of Midazolam in the [Midazolam PBPK model](https://github.com/Open-Systems-Pharmacology/OSP-PBPK-Model-Library/tree/master/Midazolam).) These "Simulation parameters" are stored in the PV BB.

- All parameters of the spatial structure that are **present in all species** and have the **same value or the same formula in all species and individuals** are stored directly in the spatial structures BB with the fixed value or an explicit formula. Example: `Organism|Thickness (endothelium)` (constant value) or `Organism|Weight of blood organs` (sum formula).

- Parameters that are present only for certain species (e.g., `Organism|Lumen|Duodenum|Thickness_p1` is only present in the human species, but not in rat, `Organism|Lumen|Duodenum|Default thickness of gut wall` is present in the rat but not in human), **or** have different species-specific (`Organism|Acidic phospholipids (blood cells) [mg/g] - RR`) values **or** vary within a population (e.g., `Orgnanism|Fat|Volume`) are **not** located in the spatial structures BB, but only in the individual. Such parameters are added to the simulation structure during the simulation creation step.

### Extension modules
Each MoBi project may contain any number of **Extension modules**. An Extension module can add or modify any part of the default PK-Sim structure - spatial structures, molecules, reactions, etc.

When adding new compartments (e.g., adding a new organ) or molecules in an Extension module, it is important to keep in mind that molecules will only be created in compartments if entries for the molecule-container combination is present in *any* IC BB. Therefore, when adding a molecule or a container in an Extension module, the IC BB should be extended accordingly.

*Creation* of new modules can be performed from scratch ("Create new module" creates a module with empty BBs) or by cloning modules ("Clone module"). Modules can be saved as .pkml file, and BBs in a module can be loaded from .pkml files.

## Combining the modules

Multiple modules can be combined to create a simulation. For simplicity, a specific combination of modules is called **model configuration**. When creating a simulation from modules, the Extension modules will *extend* or *overwrite* the selected PK-Sim modules. The user can select multiple PK-Sim modules, or PK-Sim modules in combination with Extension modules, or only the Extension module(s). The selection of the modules results in a *hierarchy* of the modules, where the order of module selection determines the order of overwrite/extend actions. I.e., multiple PK-Sim modules are always extended to a common PK-Sim module, the selected Extension module 1 extends/overwrites the common PK-Sim module, the selected Extension module 2 overwrites/extends the combination of the common PK-Sim module and the Extension module 1, and so on.

If a model configuration contains a PK-Sim module, the user must select an Individual BB and might include Expression Profile BBs. For each module that contains at least one IC or PV BB, the user can select one (or none) IC and/or PV BB.

In the hierarchy of the selected modules, the Individual BB always comes before the Extension modules (i.e., the parameters are applied to the standard PK-Sim module). **PLEASE ADD MORE CONCRETE INFORMATION ON WHEN THE INDIVIDUAL BB IS APPLIED**

## Combination rules

During simulation creation, the modules are combined to a common model structure. Entities are combined (extended or overwritten) by their absoulte path (for containers in the spatial structure, parameters, and molecule values) or by their names (for neighborhoods, passive transports, molecules, etc). The result is a simulation which represents the combination of the selected modules.

There are two types of merge behavior that can be defined for a module - "overwrite" or "extend". The following sections describe how different building blocks are applied and merged and what are the differences between "overwrite" and "extend", if any.

### Spatial structure

#### Merge behavior "Overwrite"
When combining modules "A" and "B" (with the hierarchy "A" <- "B"), all containers from module "B" will overwrite the containers with the same path in module "A". When overwriting, all descendants of a container are removed if not present in the module that overwrites (i.e., replacement of the whole tree structure). I.e., if module "A" has a container "Organism|Container 1" with two sub-containers "Container 2" and "Container 3" (absolute paths: "Organism|Container 1|Container 2" and "Organism|Container 1|Container 2"), and module "B" has a container "Organism|Container 1" without any sub-containers, the final model will contain "Organism|Container 1" without the sub-containers "Container 2" and "Container 3". "Organism|Container 1" will only have parameters that are defined in module "B", but not in module "A".

Neighborhoods will be overwritten by their names.

#### Merge behavior "Extend"
When combining modules "A" and "B" (with the hierarchy "A" <- "B"), all containers and parameters from module "B" will be added to module "A". I.e., if module "A" has a container "Organism|Container 1" with two sub-containers "Container 2" and "Container 3" (absolute paths: "Organism|Container 1|Container 2" and "Organism|Container 1|Container 3"), and module "B" has a container "Organism|Container 1" without any sub-containers, the final model will contain "Organism|Container 1" with parameters defined in both modules, and with the sub-containers "Organism|Container 1|Container 2" and "Organism|Container 1|Container 3".

- **Parameters** are always overwritten. If both modules have a parameter "Organism|Container 1|Param", the parameter from module "B" will be used
- **Tags** will be extended. If "Organism|Container 1" has a tag "Tag A" in module "A" and a tag "Tag B" in module "B", in the final model, "Organism|Container 1" will have tags "Tag A" and "Tag B".
- **Container types** will be overwritten. If "Organism|Container 1" is "physical" in module "A" and "logical" in module "B", the container will be "logical" in the final model.
- **Neighborhoods** are extended (e.g., addition of new parameters).
- **Formulas** are never overwritten (also not in the "overwrite" mode). Meaning, if module "B" defines a formula with the same name as a formula in module "A", the updated formula will only be used for the parameters defined in module "B", but not for other parameters that use this formula.

### Reactions

Reactions are always overwritten by name. Therefore, if a reaction is defined in an Extension module, it must contain all required parameters. It is not possible to only add some parameters (in merge behavior "Extend"), while retaining parameters from the module that is higher in the hierarchy.

### Passive transports

When combining passive transports, their "Kinetic" and "Parameters" properties are always overwritten by transport name. Source, target, and Include/Exclude lists for molecules are always combined. (to be extended 2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/61)

### Observers

Observers are always overwritten by name. The "In container with" and "Include/Exclude molecules" lists are always combined.

### Events

Events are always overwritten by name. 2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/62

### Parameter values

The final values of the parameters are determined in the following order:

1. **Values defined in the Building block.**  First, the value that is defined in the BB where the parameter is defined. E.g., `CYP3A4|Reference concentration` is set to 1 µmol/l in the Molecules BB. If a simulation is created only with the module with this Molecules BB without selection of an Expression Profile or a PV BB, the value will be 1 µmol/l.
2. **Values defined in an Expression Profile.** If an expression profile is selected, the values from the expression profile are applied. E.g., `CYP3A4|Reference concentration` is set to 4.32 µmol/l in the Molecules BB. If a simulation is created with the module with the Molecules BB and the Expression Profile, the value will be 4.32 µmol/l.
3. **Values defined in the PV BBs.** If a module is selected that contains a PV BB, the values from the PV BB are applied. If multiple modules have PV BBs with entries for the same parameters, the value from the latest module is selected. E.g., if the Extension module has an entry for `CYP3A4|Reference concentration` with the value of 2 µmol/l, and a simulation is created with the module with the Molecules BB where the value is 1 µmol/l, the Expression Profile where the value is 4.32 µmol/l, and the PV BB where the value is 2 µmol/l, the value in the simulation will be 2 µmol/l.

FOR PARAMETERS DEFINED IN THE INDIVIDUAL - 2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/60

### Initial conditions

The final start values for molecules are determined in the following order:

1. **Values defined in the Expression Profiles.** For proteins, values (including the formulas for start values) that are defined in the Expression Profile of the protein.
2. **Values defined in the IC BBvs.** If multiple modules have IC BBs with entries for the same molecules, the value from the latest module is selected. If an IC BB have entries for the same molecule/container combination as an Expression Profile, the values from the IC BB will be applied.

2DO - not working!! - https://github.com/Open-Systems-Pharmacology/MoBi/issues/1463