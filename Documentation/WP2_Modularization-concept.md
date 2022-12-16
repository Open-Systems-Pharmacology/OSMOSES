The following document specifies the new concept of organizing modeling projects in modules to achieve better flexibility and re-usability in MoBi.

The new concept introduces changes to the way how model structures are organized and combined to simulations. The current implementation of the software (OSPS V11.1) has two major layers of organization of a MoBi project – **Building Blocks** (BB) that are combined to **Simulations**. The proposed modularization concept extends the structure to Modules, Building Blocks, Model Configurations, and Simulations.

## Definitions:
- **Entity**: everything within the model structure - molecule, container, parameter, parameter value...
- **Building Block (BB)**: remains as in current OSPS logic - a summary of specific model parts.
- **Module**: A set of BB
  - _Extension module_: (xModule) Editable modules that can be combined with PK-Sim modules in a _Model Configuration_.
  - _PK-Sim module_: A module based on a PK-Sim model. Not editable by default and can be converted into an xModule.

- **PK-Sim snapshot**: part of the PK-Sim module allowing the re-creation of the this PK-Sim module.
- **Model configuration**: combination of modules.
- **Model**: An instance of a model configuration. Models from the same configuration may differ in their parametrization.
- **Simulation**: Combination of the **model** and the results of a simulation of this model.
- **Individual**: Parameter set describing the physiological properties of an individual. The parameter set referred to is limited to the parameters provided by PK-Sim (and, if extension of the SS require, some additional parameters).
- **MoBi-project**: A MoBi-file (e.g. *.mbp3) containing modules, model configurations, models, observed data, etc.
- **Initial Conditions (IC) BB**: formerly known as Molecular Start Values. An n x m list, with n the number of (floating) molecules and m the number of containers, holding start values of molecules, the corresponding scaling factors, and the "negative values allowed" property. If an entry for molecule `a` in container `b` is not in the list, the molecule is "not present" in this container.
- **Parameter Values (PV) BB**: formerly known as Parameter Start Values. Values of parameters (global and local). Only list parameters (including ODE-based) whose values are different from the original value in the BB.

## Project structure
A MoBi-project contains a set of:
- PK-Sim modules
- Extension modules
- Individual BBs
- Expression Profile BBs
- Model Configurations, which are combinations of (0-n) PK-Sim modules, (0-n) extension modules, (0-1) individual BB, and (0-n) expression profiles.
- Models resp. simulations, which are instances of model configurations.

![User interface of a MoBi project - expression profiles missing!](Figures/ProjectUI_Mockup.png)

## Modules
Every module consists of **building blocks (BB)**, with BB types *Spatial Structures (SS)*, *Molecules*, *Reactions*, *Passive Transports (PT)*, *Observers*, *Events*, *Parameter Values (PV)*, and *Initial conditions (IC)*. Every module inlcudes exactly one BB of each type. Empty BBs are allowed.

| Module BBs         | Comments                   |
| ------------------ | -------------------------- |
| Spatial Structure  | As it was before           |
| Molecules          | As it was before           |
| Reactions          | As it was before           |
| Passive Transports | As it was before           |
| Observers          | As it was before           |
| Events             | As it was before           |
| PV                 | defined above              |
| IC                 | defined above              |

## PK-Sim modules
A project in MoBi can be based on a PBPK model exported from PK-Sim. This model is present as a **PK-Sim module** and contains *all of the BB types*. PK-Sim modules cannot be edited by default. If the user decides to edit a PK-Sim module, the PK-Sim module will be converted to an extension module. A project can contain multiple or no PK-Sim modules. Every PK-Sim module will contain the *snapshot* of the PK-Sim model that will allow recreating of the model in PK-Sim.

It is possible to save the snapshot associated with a PK-Sim module (e.g. right-click "Save PK-Sim snapshot") so the model can be recreated in PK-Sim.

Export of a PK-Sim model to MoBi creates one PK-Sim module, one individual, and (0-n) expression profile BBs.

When converting a PK-Sim module into an extension module (i.e., making the PK-Sim module editable), the user has to select an Individual based on which the missing values will be filled out.

When viewing a SS BB of a PK-Sim module, one of the existing Individuals can be selected fron the drop-down list.

## Extension modules
Each MoBi project contains any number of **Extension modules**.

Each module containing molecules automatically contains default IC entries for these molecules in the PK-Sim module(s) or the combination of all PK-Sim modules with the SS BB within the module.

*Creation* of new modules can be performed from scratch ("Create new module" creates a module with empty BBs) or by copying whole modules ("Clone module") or parts of the modules (e.g., multi-select SS and reactions BB and select "Create new module from..."). It is furthermore possible to save modules (as pkml), or save BB and load them in modules.

## Deletion of entities
An extension module does not only allow to _extend_ or _overwrite_ the entities of modules it is combined with (e.g., PK-Sim modules), but also to _delete_ certain parts present in other modules. This functionality will be implemented with **Deletion-Lists** as part of the BBs within the extension modules. A deletion-list holds full paths to the entities that will be deleted from the model structure. Every deletion is recursive, i.e., all children of an entity specified in the deletion-list will also be removed from the final structure.

The user can create entries either by typing the full path or by selecting them from a tree.

- For SS BB, containers or neighborhoods can be added to the list.
    - Tags? E.g. "Organism|Liver|TAGS|LiverTag"
- For molecules, molecules as such can be added to the list.
- For reactions, the reactions can be added to the list.
- For passive transports, either single PT, or
    - _Conditions_ of certain transports, e.g., `"DrugAbsorption_para|SOURCE|Lumen"`, `"DrugAbsorption_para|TARGET|Mucosa"`, `"DrugAbsorption_para|INCLUDE|Glucose"`, `"DrugAbsorption_para|EXCLUDE|Insulin"`. If these entries are added to the "Remove"-list, the respective conditions will be removed from the existing transports.
    - Similarly, for PT we could introduce the "Add"-list to add conditions
- For observers, the observerst themselves and the conditions similarly as for the PT
- For Events, full path to the container that should be removed.
- No "Delete" for IC and PV
- No "Delete" for the "Individuals"- and "Expression Profiles"-BB.

In case an entry defined in the "Delete"-list is not found, the model configuration should be created and a warning generated.

## Model configuration

Specific combination of modules is called **model configuration**. Upon building the model configuration, the extension modules will *extend* or *overwrite* the selected PK-Sim modules. The user is allowed to select multiple PK-Sim modules, or PK-Sim modules in combination with extension modules, or only the extension module(s). The number of modules selected is not limited. The selection of PK-Sim and extension modules results in an *hierarchy* of the modules within the build configuration.

![Model configuration creation dialog](Figures/ModelConfiguration_Mockup.png)

Multple PK-Sim modules are always extended to a common PK-Sim module. Selected extension module 1 extends/overwrites the common PK-Sim module, selected extension module 2 overwrites/extends the combination of the common PK-Sim module and module 1, and so on. Internally, the software will create new BBs from the combination of the modules and create the models applying the current logic.

If a model configuration contains a PK-Sim module, the user must select an Individual (pre-select the first one?) and Expression Profiles (optionally). Individual and Expression Profiles can be selected without a PK-Sim module. In the hierarchy of the selected modules, the Individuals-BB always comes before the extension modules (i.e., the parameters are applied to the standard PK-Sim module).

The model configuration will potentially contain IC values that are not defined in the modules (e.g., a combination of a module with new molecules with a module with a new organ). These values are populated with default values (initial condition e.g. 0). The newly created BBs can be exported and re-imported as new modules, resulting in modules containg the intial conditions for these non-standard combinations.

Model configurations can be **configured** and **cloned**.

From model configurations, **models** can be created. A single model configuration can have multiple models. All these models will have the same structure, but may differ in their parameters and/or initial conditions. During creation of a model from the model configuration, the user can select a different Individual and/or Expression Profile.

*Batch actions* such as "run all", "update all" will be available as an option in the context menu of model configuration.

## Extension rules
Combinations of BBs will be created by the following logic:
1. For each entity in module 1:
* If the entity is not present in the common PK-Sim module, add the entity to the new combination
* If the entity is present in the common PK-Sim module module, use the instance defined in module 1 in the new combination
2. Repeat 1. for each module as defined in the hierarchy of the selected modules
3. Create the final combination of the BBs as a model configuration.

Deletions are performed according to the hierarchy of the selected modules. I.e., if extension module 1 has an entry "Organism|Kidney" in the "Deletion"-list, and extension module 2 has a container "Organism|Kidney" (extended kidney), the final model configuration **will** have a kidney, because module 2 is applied after module 1.

## Commit changes
Commit to module will replace commit to BB. Only commiting changed values of parameters will be supported. The changed value will be propagated to the PV of the last module where the respective parameter is present. E.g., if Parameter A is present in Modules 4, 2, and any PK-Sim module (the number corresponds to the hierarchy selected during model configuration creation), the value will be propagated to Module 4. _Alternatively_: Show dialogue to select to which module the changes should be commited to.

## Administration protocols
MoBi should provide a wizard similar to that in PK-Sim to allow convenient creation of administration protocolls. ~~Combined with the **Generic modules** as described above, the administration protocolls could be stored as separate modules and applied repeatedly during model configuration creation. Only possible if _Alternative 2_ for generic modules is implemented.~~

## Individuals
See ["Individuals-BB"](Individuals-BB.md).

## Protein Expression BB
See ["Expression Profile-BB"](ExpressionProfile.md).

## PK-Sim model to MoBi Project
[Export Model to MoBi](WP2.1x_Export-PKSim-to-MoBi.md)

## Populations
Models can be exported to PK-Sim for population simulations.
