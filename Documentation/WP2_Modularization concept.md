Modularization concept for MoBi.

## Definitions:
- **Entity**: everything within the model structure - molecule, container, parameter, parameter value...
- **Building Block (BB)**: remains as in current OSPS logic - a summary of specific model parts.
- **Module**: A set of BB
  - _PK-Sim module_: A module based on a PK-Sim model. Not editable by default and will turn into an xModule if "set to" _editable_. May contain the whole PBPK model or just some modules (_what is meant by "just some modules"?_).
  - _Extension module_: (xModule) Editable modules that can be combined with PK-Sim modules in a _Model Configuration_.
- **PK-Sim snapshot**: part of the PK-Sim module allowing the re-creation of the this PK-Sim module or the combination of PK-Sim modules (_why also the "combination of PK-Sim modules"?_).
- **Model configuration**: combination of modules.
- **Model**: An instance of a model configuration.
- **Simulation**: Combinatioin of the **model** and the results of a simulation of this model.
- **Individual**: Parameter set describing the physiological properties of an individual. The parameter set referred to is limited to the parameters provided by PK-Sim (and, if extension of the SS require, some additional parameters), it should _not_ describe e.g. different disease states.
- **MoBi-project**: A MoBi-file (e.g. *.mbp3) containing modules, model configurations, models, observed data, etc.
- **Initial Conditions (IC) BB**: formerly known as Molecular Start Values. An n x m list, with n the number of (floating) molecules and m the number of containers, holding start values of molecules, the corresponding scaling factors, and the "negative values allowed" property. If an entry for molecule `a` in container `b` is not in the list, the molecule is "not present" in this container.
- **Parameter Values (PV) BB**: formerly known as Parameter Start Values. Values of parameters (global and local). Only list parameters (including ODE-based) whose values are different from the original value in the BB.

## Project structure
A MoBi-project is based on **modules**. Every module consists of **building blocks (BB)**, with BB types *Spatial Structures (SS)*, *Molecules*, *Reactions*, *Passive Transports (PT)*, *Observers*, *Events*, *Parameter Values (PV)*, and *Initial conditions (IC)*. Every module inlcudes exactly one BB of each type. Empty BBs are allowed.


| Module BBs         | Comments                   |
| ------------------ | -------------------------- |
| Spatial Structure  | As it was before           |
| Molecules          | As it was before           |
| Reactions          | As it was before           |
| Passive Transports | As it was before           |
| Observers          | As it was before           |
| Events             | As it was before (new UI?) |
| PV                 | defined above              |
| IC                 | defined above              |

## PK-Sim modules
A project in MoBi can be based on a PBPK model exported from PK-Sim. This model is present as a **PK-Sim module** and contains *all of the BB types*. A model exported from PK-Sim can result in multiple PK-Sim modules in MoBi - a module with the basic PBPK model and extensions like the pregnancy module. The basic PK-Sim module must be sufficient for creating a valid model configuration. PK-Sim modules cannot be edited by default. If the user decides to edit a PK-Sim module, the PK-Sim module will be converted to an extension module. A project can contain multiple or no PK-Sim modules. Every PK-Sim module will contain the *snapshot* of the PK-Sim model that will allow recreating of the model in PK-Sim. As one PK-Sim model can result in multiple PK-Sim modules in MoBi, the same snapshot can be assigned to multiple modules.

It is possible to save the snapshot associated with a PK-Sim module (e.g. right-click "Save PK-Sim snapshot") so the model can be recreated in PK-Sim.

Can we have more than one PK-Sim module?
- Technically yes, and can be **merged as we know it**

## Extension modules
Each MoBi project contains any number of **Extension modules**.

Each module containing molecules automatically contains default IC entries for these molecules in the PK-Sim module(s) or the combination of all PK-Sim modules with the SS BB within the module.

Each SS defines default values of "isPresent" and "Negative values allowed" for certain types of molecules (floating compounds, enzymes, transporters) (selctable via a check-box). A SS BB within an extension module may contain containers that are _not connected_ with a common parent container. Example - a tumor organ and a kidney organ. Each top-level container within a sub-module ("Tumor" and "Kidney") **must** have a property "Parent container", which is the full path of the container in the hierarchy. During combination of the modules, containers will be placed as direct children of the specified parent container.

*Creation* of new modules can be performed from scratch ("Create new module" creates a module with empty BBs) or by copying whole modules ("Clone module") or parts of the modules (e.g., multi-select SS and reactions BB and select "Create new module from..."). It is furthermore possible to save modules (as pkml), or save BB and load them in modules.

## Neighborhoods
To allow replacement or addition of structures to the spatial structure (e.g., adding tumor, or replacing the kidney) within modules, the module has to come with the information about how the organ (or just some containers) are connected to the whole organism. Therefore:

- It should be possible to create neighborhoods by other means than drag-and-drop. E.g., a UI "Create neighborhood", where the user selects (or types in the full path to) the neighbors. In case of a tumor module, the user would select "PARENT|Tumor|Plasma" as one neighbor, and type in "Organism|VenousBlood|Plasma" as the second neighbor. the "PARENT keyword is necessary as the path within the module is not complete.

A neighborhood is described by:
- Name
- ID
- Full path to the first neighbor
- Full path to the second neighbor
- Parameters
- Sub-container "MoleculeProperties"
  - Parameters

Each spatial structure BB within a module has a list of neighborhoods. Combining modules will overwrite neighborhoods with identical neighbors (i.e., matching of neighborhoods is performed by their neighbors and not by names). Deleting of neighborhoods is possible by defining them in the "Delete"-list of the SS BB.

Exporting of a container to a PKML also exports all its sub-containers and all neighborhoods that have this container or his sub-containers as a neighbor.

If an extension module defines a neighborhood with a neighbor that is not present in the model configuration, the model configuration is created and warnings are displayed ("WARNING: The neighborhood "Tumor_pls_Head_pls" was not created, as the neighbor "Organism|Head|Plasma" is not present!").

## Deletion of entities
An extension module does not only allow to _extend_ or _overwrite_ the entities of modules it is combined with (e.g., PK-Sim modules), but also to _delete_ certain parts present in other modules. This functionality will be implemented with **Deletion-Lists** as part of the BBs within the extension modules. A deletion-list holds full paths to the entities that will be deleted from the model structure. Every deletion is recursive, i.e., all children of an entity specified in the deletion-list will also be removed from the final structure.

The user can create entries either by typing the full path or by selecting them from a tree.

- For SS BB, containers or neighborhoods can be added to the list.
- For molecules, molecules as such can be added to the list.
- For reactions, the reactions can be added to the list.
- For passive transports, either single PT, or
-- _Conditions_ of certain transports, e.g., `"DrugAbsorption_para|SOURCE|Lumen"`, `"DrugAbsorption_para|TARGET|Mucosa"`, `"DrugAbsorption_para|INCLUDE|Glucose"`, `"DrugAbsorption_para|EXCLUDE|Insulin"`. If these entries are added to the "Remove"-list, the respective conditions will be removed from the existing transports.
-- Similarly, for PT we could introduce the "Add"-list to add conditions
- For observers, the observerst themselves and the conditions similarly as for the PT
- For Events, full path to the container that should be removed.
- No "Delete" for IC and PV
- No "Delete" for the "Individuals"-BB.

In case an entry defined in the "Delete"-list is not found, the model configuration should be created and a warning generated.

## Model configuration

Specific combination of modules is called **model configuration**. Upon building the model configuration, the extension modules will *extend* or *overwrite* the selected PK-Sim modules. The user is allowed to select multiple PK-Sim modules, or PK-Sim modules in combination with extension modules, or only the extension module(s). The number of extension modules selected is not limited. The selection of PK-Sim and extension modules results in an *hierarchy* of the modules within the build configuration. Multple PK-Sim modules are always extended to a common PK-Sim module. Selected extension module 1 extends/overwrites the common PK-Sim module, selected extension module 2 overwrites/extends the combination of the common PK-Sim module and module 1, and so on. Internally, the software will create new BBs from the combination of the modules and create the models applying the current logic. Combinations of BBs will be created by the following logic:
1. For each entity in module 1:
* If the entity is not present in the common PK-Sim module, add the entity to the new combination
* If the entity is present in the common PK-Sim module module, use the instance defined in module 1 in the new combination
2. Repeat 1. for each module as defined in the hierarchy of the selected modules
3. Create the final combination of the BBs as a model configuration.

Deletions are performed according to the hierarchy of the selected modules. I.e., if extension module 1 has an entry "Organism|Kidney" in the "Deletion"-list, and extension module 2 has a container "Organism|Kidney" (extended kidney), the final model configuration **will** have a kidney, because module 2 is applied after module 1.

If a model configuration contains a PK-Sim module, the user has to select an Individual and an Expression Profile (optionally).

The model configuration will potentially contain IC values that are not defined in the modules (e.g., a combination of a module with new molecules with a module with a new organ). These values are populated with default values (initial condition e.g. 0). The newly created BBs can be exported and re-imported as new modules, resulting in modules containg the intial conditions for these non-standard combinations.

Model configurations can be **configured** and **cloned**.

From model configurations, **models** can be created. A single model configuration can have multiple models. All these models will have the same structure, but may differ in their parameters. During creation of a model from the model configuration, the user can select a different Individual and/or Expression Profile.

*Batch actions* such as "run all", "update all" will be available as an option in the context menu of model configuration.

## Commit changes
Commit to module will replace commit to BB. Only commiting changed values of parameters will be supported. The changed value will be propagated to the PV of the last module where the respective parameter is present. E.g., if Parameter A is present in Modules 4, 2, and any PK-Sim module (the number corresponds to the hierarchy selected during model configuration creation), the value will be propagated to Module 4. _Alternatively_: Show dialogue to select to which module the changes should be commited to.

## Generic modules
Some modules should be applied to not specified molecules. As an example, a module of binding to Albumin with dynamic albumin PBPK. Such a module would provide albumin PBPK parts (i.e., molecules, reactions, and IC) and reactions (and potentially parameter values) that are generic for the binding partner. Generic molecules are defined by using naming convention such as _MOLECULE_yourmoleculename1_, _MOLECULE_yourmoleculename2_, etc. A reaction defining the binding of a molecule to albumin will have _MOLECULE_yourmoleculename1_ and albumin (provided within the module) as educts, and _MOLECULE_yourmoleculename2_ as product.

The concept of generic keywords also allows to overwrite values of parameters / properties of molecues, e.g., resetting the value of "Fraction unbound" to 1 for the dynamic albumin binding module. In the PV of the module, the path to the parameter will read "_MOLECULE_yourmoleculename1_|Fraction unbound".

During creation of model configuration, the user has to select which molecule from the modules will correspond to _MOLECULE_yourmoleculename1_ and _MOLECULE_yourmoleculename2_. This allows to re-utilize the generic modules by selecting it multiple times in model configuration creation. For each selected instance, the user will select the molecules corresponding to the generic names. To differentiate between module that can be selected multiple times (only makes sence for generic modules) and those can be selected only once, a module will have a property "isMultipleSelectionAllowed", defined by a check-box. The drawback of this solution is that the concept does not allow creation of new molecules, e.g. generic products. This could be possible to implement but would require additional syntax - we will have to differentiate between generic molecules that will be selected upon model configuration creation (e.g., educt of the binding reaction), and the generic names of _newly created_ molecules (e.g., the product).

Alternatively to the multiple selection of a submodule: multiple REPLACEMENT of molecule names. Thus instead of 1:1 matching {Molecule1;Molecule2}==>{A;B} it would be possible to match {Molecule1;Molecule2}==>{A;B}, {C;D}, {E;F} ...

## Administration protocols
MoBi should provide a wizard similar to that in PK-Sim to allow convenient creation of administration protocolls. Combined with the **Generic modules** as described above, the administration protocolls could be stored as separate modules and applied repeatedly during model configuration creation. Only possible if _Alternative 2_ for generic modules is implemented.

## Individuals
A new BB-type **Individuals** will be introduced. This BB is not part of a module but will be shared within the project. The entries of this BB contain spatial structure parameter sets describing individual physiology. The respective parameters in the spatial structure BB will be filled out with "NaN"-values. When converting a PK-Sim module into an extension module (i.e., making the PK-Sim module editable), the user has to select an Individual based on which the missing values will be filled out.

In addition to parameter values, each Individual-entry stores the information needed to re-create the specific individual. The information is:
- Species
- Population
- Gender
- Age
- Weight
- Height
- User-defined Anatomy & Physiology (as in the corresponding PK-Sim tab).

Upon creation of a Model Configuration,  an individual **must** be selected if a PK-Sim module is used. If no PK-Sim modules are used (e.g., a QSP model or a PBPK model using a PBPK module that has been changed and is no longer a valid PK-Sim module). In the hierarchy of the selected modules, the Individuals-BB always comes before the extension modules (i.e., the parameters are applied to the standard PK-Sim module (incl. PK-Sim extension modules, if applicable)).

An Individual-BB is always created upon import of a PK-Sim model based on the information stored in the "Individuals"-part of the PK-Sim snapshot. Each BB is **read-only** by default, the property that allows the re-qualification of projects. If the user decides to directly edit the values of the individual-BB, the BB becomes "non-qualifiable". Each "qualifiable" BB gets a snapshot that includes the information necessary to re-create this particular individual (compare to PK-Sim individuals).

It is possible to create Individuals with an interface similar to that from PK-Sim, where parameter sets are generated from the PK-Sim database.

To allow **aging**, this property must be selected upon creation of the individual in PK-Sim. In this case, the corresponding parameters well be represented by table formulas.

## Protein Expression BB
Expression of proteins (i.e., non-floating molecules) will be defined in the Protein-Expression BB. As Individual-BB, the Protein expression BB is not a part of any module. Every model exported from PK-Sim creates a Protein Expression BB.

Non-floating molecules are therefore not part of the IC-BB of any module.

The exact structure is to be discussed.

## PK-Sim model to MoBi Project
[Export Model to MoBi](https://github.com/Open-Systems-Pharmacology/OSMOSES/wiki/WP2.1x:-Export-Model-to-MobI)

## Populations
Models can be exported to PK-Sim for population simulations.