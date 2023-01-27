## Collection of topics that were discussed but will not be part of the first implementation.

### Population simulations in MoBi
MoBi could provide infrastructure for performing population simulations similar to that in PK-Sim.

### Extraction of all paramaters from selected BB of a module into PV of a new module
Example: Right-mouse click on a BB in any module -> Save all parameters to pkml, and the pkml can be loaded as the PV-BB in any module.

### Generic definition of the parent container of modules
Currently, a container provided within an extension module has to specify the full path to the parent container it will be attached to.
It could be a container criteria potentially allowing to attach the substructure at more than one place in the parent-SS. Neighborhoods could be defined by container criteria as well. E.g "PARENT_CONTAINER/Plasma"

### Commit structural changes from models to modules
If commiting changes in structure (i.e., the structure was changed in the original modules (e.g., added a reaction) and should be restored from the simulation), show dialogue to select to which module to propagate the changes. After commiting structural changes, all models of the configuration must be re-configured to restore structural identity!

### PK-Sim modules
A model exported from PK-Sim can result in multiple PK-Sim modules in MoBi - a module with the basic PBPK model and extensions like the pregnancy module.
As one PK-Sim model can result in multiple PK-Sim modules in MoBi, the same snapshot can be assigned to multiple modules.

## Generic modules
Some modules should be applied to not specified molecules. As an example, a module of binding to Albumin with dynamic albumin PBPK. Such a module would provide albumin PBPK parts (i.e., molecules, reactions, and IC) and reactions (and potentially parameter values) that are generic for the binding partner. Generic molecules are defined by using naming convention such as _MOLECULE_yourmoleculename1_, _MOLECULE_yourmoleculename2_, etc. A reaction defining the binding of a molecule to albumin will have _MOLECULE_yourmoleculename1_ and albumin (provided within the module) as educts, and _MOLECULE_yourmoleculename2_ as product.

The concept of generic keywords also allows to overwrite values of parameters / properties of molecues, e.g., resetting the value of "Fraction unbound" to 1 for the dynamic albumin binding module. In the PV of the module, the path to the parameter will read "_MOLECULE_yourmoleculename1_|Fraction unbound".

During creation of model configuration, the user has to select which molecule from the modules will correspond to _MOLECULE_yourmoleculename1_ and _MOLECULE_yourmoleculename2_. This allows to re-utilize the generic modules by selecting it multiple times in model configuration creation. For each selected instance, the user will select the molecules corresponding to the generic names. To differentiate between module that can be selected multiple times (only makes sence for generic modules) and those can be selected only once, a module will have a property "isMultipleSelectionAllowed", defined by a check-box. The drawback of this solution is that the concept does not allow creation of new molecules, e.g. generic products. This could be possible to implement but would require additional syntax - we will have to differentiate between generic molecules that will be selected upon model configuration creation (e.g., educt of the binding reaction), and the generic names of _newly created_ molecules (e.g., the product).

Alternatively to the multiple selection of a submodule: multiple REPLACEMENT of molecule names. Thus instead of 1:1 matching {Molecule1;Molecule2}==>{A;B} it would be possible to match {Molecule1;Molecule2}==>{A;B}, {C;D}, {E;F} ...