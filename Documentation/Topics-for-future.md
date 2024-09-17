## Collection of topics that were discussed but will not be part of the first implementation.

### Population simulations in MoBi
MoBi could provide infrastructure for performing population simulations similar to that in PK-Sim.

### Extraction of all paramaters from selected BB of a module into PV of a new module
Example: Right-mouse click on a BB in any module -> Save all parameters to pkml, and the pkml can be loaded as the PV-BB in any module.

### Generic definition of the parent container of modules
Currently, a container provided within an Extension module has to specify the full path to the parent container it will be attached to.
It could be a container criteria potentially allowing to attach the substructure at more than one place in the parent-SS. Neighborhoods could be defined by container criteria as well. E.g "PARENT_CONTAINER/Plasma"

### Commit structural changes from models to modules
If commiting changes in structure (i.e., the structure was changed in the original modules (e.g., added a reaction) and should be restored from the simulation), show dialogue to select to which module to propagate the changes. After commiting structural changes, all models of the configuration must be re-configured to restore structural identity!

### PK-Sim modules
A model exported from PK-Sim can result in multiple PK-Sim modules in MoBi - a module with the basic PBPK model and extensions like the pregnancy module.
As one PK-Sim model can result in multiple PK-Sim modules in MoBi, the same snapshot can be assigned to multiple modules.

Every PK-Sim module will contain the *snapshot* of the PK-Sim model that will allow recreating of the model in PK-Sim. It is possible to save the snapshot associated with a PK-Sim module (e.g. right-click "Save PK-Sim snapshot") so the model can be recreated in PK-Sim. Converting a PK-Sim module to an Extension module deletes the snapshot.

When converting a PK-Sim module into an Extension module (i.e., making the PK-Sim module editable), the user has to select an Individual based on which the missing values will be filled out.

When viewing a SS BB of a PK-Sim module, one of the existing Individuals can be selected fron the drop-down list.

## Generic modules
Some modules should be applied to not specified molecules. As an example, a module of binding to Albumin with dynamic albumin PBPK. Such a module would provide albumin PBPK parts (i.e., molecules, reactions, and IC) and reactions (and potentially parameter values) that are generic for the binding partner. Generic molecules are defined by using naming convention such as _MOLECULE_yourmoleculename1_, _MOLECULE_yourmoleculename2_, etc. A reaction defining the binding of a molecule to albumin will have _MOLECULE_yourmoleculename1_ and albumin (provided within the module) as educts, and _MOLECULE_yourmoleculename2_ as product.

The concept of generic keywords also allows to overwrite values of parameters / properties of molecues, e.g., resetting the value of "Fraction unbound" to 1 for the dynamic albumin binding module. In the PV of the module, the path to the parameter will read "_MOLECULE_yourmoleculename1_|Fraction unbound".

During creation of model configuration, the user has to select which molecule from the modules will correspond to _MOLECULE_yourmoleculename1_ and _MOLECULE_yourmoleculename2_. This allows to re-utilize the generic modules by selecting it multiple times in model configuration creation. For each selected instance, the user will select the molecules corresponding to the generic names. To differentiate between module that can be selected multiple times (only makes sence for generic modules) and those can be selected only once, a module will have a property "isMultipleSelectionAllowed", defined by a check-box. The drawback of this solution is that the concept does not allow creation of new molecules, e.g. generic products. This could be possible to implement but would require additional syntax - we will have to differentiate between generic molecules that will be selected upon model configuration creation (e.g., educt of the binding reaction), and the generic names of _newly created_ molecules (e.g., the product).

Alternatively to the multiple selection of a submodule: multiple REPLACEMENT of molecule names. Thus instead of 1:1 matching {Molecule1;Molecule2}==>{A;B} it would be possible to match {Molecule1;Molecule2}==>{A;B}, {C;D}, {E;F} ...

## Model configurations
- **Model configuration**: combination of modules.
- **Model**: An instance of a model configuration. Models from the same configuration may differ in their parametrization.

*Batch actions* such as "run all", "update all" will be available as an option in the context menu of model configuration.

## Read-only PK-Sim modules
PK-Sim module cannot be edited. Editing will convert the module to an Extension module, accompanied by a user warning.

## Commit changes
STILL UNDER DISCUSSION

Commit to module will replace commit to BB. Only commiting changed values of parameters will be supported. The changed value will be propagated to the PV of the last module where the respective parameter is present. E.g., if Parameter A is present in Modules 4, 2, and any PK-Sim module (the number corresponds to the hierarchy selected during model configuration creation), the value will be propagated to Module 4. _Alternatively_: Show dialogue to select to which module the changes should be commited to.

## Recreation of PK-Sim modules from snapshot

## Re-design of applications and events in MoBi

## Individuals
- Allow aging. To allow **aging**, this property must be selected upon creation of the individual in PK-Sim. In this case, the corresponding parameters well be represented by table formulas.