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