# OSMOSES modularization concept

The following document specifies the new concept of organizing modeling projects in modules to achieve better flexibility and re-usability in MoBi.

The new concept introduces changes to how model structures are organized and combined into simulations. The current implementation of the software (OSPS V11.3) has two major layers of organization of a MoBi project – **Building Blocks** (BB) that are combined into **Simulations**. The new modularization concept extends the structure to **Modules**, **Building Blocks**, and **Simulations**.

## Definitions:
- **Entity**: everything within the model structure - molecule, container, parameter, parameter value...
- **Building Block (BB)**: Specific model parts that, combined together, create a full model structure. See documentation on [building blocks](https://docs.open-systems-pharmacology.org/working-with-mobi/mobi-documentation/building-block-concepts).
- **Module**: A set of BB
  - _PK-Sim module_: A module based on a PK-Sim PBPK model. As best practice, a PK-Sim module should not be modified. Instead, all changes/extensions to the model should be done in extension modules. A PK-Sim module is converted into an extension module when edited by the user.
  - _Extension module_: (xModule) Editable modules that contain any changes to the model structure made by the user.

- **PK-Sim snapshot**: part of the PK-Sim module allowing the re-creation of the PK-Sim module.
- **Model**: A combination of modules.
- **Model structure**: Structural definition of the model, including the containers, connections, species, and active and passive processes, but excluding the parametrization and initial conditions of the final simulation.
- **Simulation**: Combination of the **model** and the results of a simulation of this model. *Note:* The distinction between a model and a simulation is not obvious in the OSPS, and these terms are interchangeable to a greater extent.
- **MoBi-project**: A MoBi-file (e.g. *.mbp3) containing modules, models, observed data, etc.
- [*Initial Conditions (IC) BB*](BuildingBlocks/InitialConditions-BB.md): formerly known as Molecule Start Values.
- [*Parameter Values (PV) BB*](BuildingBlocks/ParameterValues-BB.md): formerly known as Parameter Start Values.
- **Individual**: Parameter set describing the physiological properties of an individual. The parameter set referred to is limited to the parameters provided by PK-Sim. Technically comparable to the Parameter Values BB with additional metadata. See [*Individuals*](BuildingBlocks/Individuals-BB.md) for details.
- **Expression profile**: Parameter set describing the expression of a protein. See [*Expression Profile*](BuildingBlocks/ExpressionProfile.md) for details.

## Project structure
A MoBi-project contains a set of:
- PK-Sim modules
- Extension modules
- Individual BBs
- Expression Profile BBs
- Models resp. Simulations, which are combinations of (0-n) PK-Sim modules, (0-n) extension modules, (0-1) individual BB, and (0-n) expression profiles.

## Modules
Every module consists of **building blocks (BB)**, with BB types [*Spatial Structures (SS)*](BuildingBlocks/SpatialStructures-BB.md) (0-1), *Molecules* (0-1), *Reactions* (0-1), [*Passive Transports (PT)*](BuildingBlocks/PassiveTransports-BB.md) (0-1), *Observers* (0-1), *Events* (0-1), [*Parameter Values (PV)*](BuildingBlocks/ParameterValues-BB.md) (1-n), and [*Initial conditions (IC)*](BuildingBlocks/InitialConditions-BB.md) (1-n). Every module includes no or exactly one BB of each type, except for PV and IC BBs, of which multiple can be present in one module.

### PK-Sim modules
A project in MoBi can be based on a PBPK model exported from PK-Sim. This model is present as a **PK-Sim module** and contains *all of the BB types*. PK-Sim modules cannot be edited by default. If the user decides to edit a PK-Sim module, the PK-Sim module will be converted to an extension module. A project can contain multiple or no PK-Sim modules.

Export of a PK-Sim model to MoBi creates one PK-Sim module, one individual, and (0-n) expression profile BBs.

Compared to the previous versions, v12 introduces some changes in how and where individual parameters are stored when a PBPK model is imported in MoBi. To avoid duplication of information, the PV BB only holds values for parameters that are different from those stored in other BBs. In most cases, the PV BB will be empty when a PK-Sim model is imported into MoBi. An exception is when the user has modified parameter values in the PK-Sim simulation (e.g., intestinal permeability of Midazolam in the [Midazolam PBPK model](https://github.com/Open-Systems-Pharmacology/OSP-PBPK-Model-Library/tree/master/Midazolam).) In this case, the user-defined parameters are stored in the PV BB.

- All parameters of the spatial structure that have the **same value or the same formula in all species and individuals** are stored directly in the spatial structures BB with the fixed value or an explicit formula. Example: `Organism|Thickness (endothelium)` (constant value) or `Organism|Weight of blood organs` (sum formula).

- Parameters that are present in **all species** but have different species-specific values are located in the SS BB, but their value is set to `NaN`. Example: `Organism|Acidic phospholipids (blood cells) [mg/g] - RR`, which has a value of 0.57 for humans and 0.5 for rats. The actual value is stored in the *Individual* BB. When creating a simulation and selecting a PK-Sim module, the user must select an individual. The value from the individual BB will then be applied to the parameter and replace the `NaN` stored in the SS BB.

- **Distributed physiological parameters**. Parameters that are present only for certain species (e.g., `Organism|Lumen|Duodenum|Thickness_p1` is only present in the human species, but not in rat, `Organism|Lumen|Duodenum|Default thickness of gut wall` is present in the rat but not in human), **or** have a distribution in a species population (e.g., `Orgnanism|Fat|Volume`) are **not** located in the spatial structures BB, but only in the individual. Such parameters are added to the simulation structure during the simulation creation step. *Note:* Only parameters defined in the Individual BB are added to the model structure, but not the parameters defined in the PV BB.

### Extension modules
Each MoBi project may contain any number of **Extension modules**. An extension module can add or modify any part of the default PK-Sim structure - spatial structures, molecules, reactions, etc.

When adding new compartments (e.g., adding a new organ) or molecules in an extension module, it is important to keep in mind that molecules will only be created in compartments if entries for the molecule-container combination is present in *any* IC BB. Therefore, when adding a molecule or a container in an xModule, the IC BB should be extended accordingly.

*Creation* of new modules can be performed from scratch ("Create new module" creates a module with empty BBs) or by cloning whole modules ("Clone module"). Modules can be saved as pkml, and BBs in a module can be loaded from pkml.

## Model configuration

Specific combination of modules is called **model configuration**. Upon building the model configuration, the extension modules will *extend* or *overwrite* the selected PK-Sim modules. The user can select multiple PK-Sim modules, or PK-Sim modules in combination with extension modules, or only the extension module(s).  The selection of modules results in a *hierarchy* of the modules, where the order of module selection determines the order of overwrite/extend actions. E.g., multiple PK-Sim modules are always extended to a common PK-Sim module, elected extension module 1 extends/overwrites the common PK-Sim module, selected extension module 2 overwrites/extends the combination of the common PK-Sim module and module 1, and so on.

If a model configuration contains a PK-Sim module, the user must select an Individual and Expression Profiles (optionally). In the hierarchy of the selected modules, the Individuals-BB always comes before the extension modules (i.e., the parameters are applied to the standard PK-Sim module). **PLEASE ADD MORE CONCRETE INFORMATION ON WHEN THE INDIVIDUAL BB IS APPLIED**

For each module that contains at least one IC or PV BB, the user can select one (or none) IC and/or PV BB.

## Combination rules

During simulation creation, the modules are combined to a common model structure. Entities are combined (extended or overwritten) by their absoulte path (for containers in the spatial structure, parameters, and molecule values) or by their names (for neighborhoods, transports, molecules, etc).

### Spatial structure

For combining the spatial structures BB, two modes exist. A module can *extend* or *overwrite* the structure created in the previous steps.

### Merge behavior "Overwrite"
When combining modules "A" and "B" (with the hierarchy "A" -> "B"), all containers from module "B" will overwrite the containers with the same path in module "A". I.e., if module "A" has a container "Organism|Container A" with two sub-containers "Container B" and "Container C" (absolute paths: "Organism|Container A|Container B" and "Organism|Container A|Container C"), and module "B" has a container "Organism|Container A" without any sub-containers, the final model will contain "Organism|Container A" without the sub-containers "Container B" and "Container C". "Container A" will only have parameters that are defined in module "B", but not in module "A".

### Merge behavior "Extend"
When combining modules "A" and "B" (with the hierarchy "A" -> "B"), all containers and parameters from module "B" will be added to module "A". I.e., if module "A" has a container "Organism|Container A" with two sub-containers "Container B" and "Container C" (absolute paths: "Organism|Container A|Container B" and "Organism|Container A|Container C"), and module "B" has a container "Organism|Container A" without any sub-containers, the final model will contain "Organism|Container A" with parameters defined in both modules, and with the sub-containers "Container B" and "Container C".

- Parameters will be overwritten. If both modules have a parameter "Organism|Container A|Param", the parameter from module "B" will be used
- Tags will be extended. If "Organism|Container A" has a tag "Tag A" in module "A" and a tag "Tag B" in module "B", in the final model, "Organism|Container A" will have tags "Tag A" and "Tag B".
- Container types will be overwritten. If "Organism|Container A" is "physical" in module "A" and "logical" in module "B", the container will be "logical" in the final model.

**Containers** are extended or overwritten by their full path. When overwriting, all descendants of a container are removed if not present in the module that overwrites (i.e., replacement of the whole tree structure).

**Tags** of containers or parameters are extended or overwritten, depending on the selected merge behavior.

**Neighborhoods** are extended (e.g., addition of new parameters) or overwritten by the neighborhood name.

**Formulas** are never overwritten. Meaning, if module "B" defines a formula with the same name as a formula in module "A", the updated formula will only be used for the parameters defined in module "B", but not for other parameters that use this formula.

### Reactions

Reactions are always overwritten by name. Therefore, if a reaction is defined in an extension module, it must contain all required parameters. It is not possible to only add some parameters (in merge behavior "Extend"), while retaining parameters from the module that is higher in the hierarchy.

### Passive transports

When combining passive transports, their "Kinetic" and "Parameters" properties are always overwritten by transport name. Source, target, and Include/Exclude lists for molecules are always combined. (to be extended 2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/61)

### Observer

Observers are always overwritten by name. The "In container with" and "Include/Exclude molecules" lists are always combined.

### Events

Events are always overwritten by name. 2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/62
