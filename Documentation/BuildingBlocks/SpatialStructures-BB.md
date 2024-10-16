## Spatial Structures (SS) Building Block (BB)

### Parent path
The spatial structures (SS) of the models are organized in a hierarchy of containers, where each container can be a *child* of a *parent* container. At the same time, `Container A` can be a *child* of maximal one *parent container* `Container B` and the *parent* of multiple containers. Each container has a property `Parent path` which specifies the **full path** to its parent container.

In a module, the user can change the parent container property for the top level containers. When combining modules, the containers (and all their children) of the module will be placed under the container specified in the `Parent path` property. If the parent path of `Container A` is empty OR the container with the specified parent path is not found in the simulation, `Container A` will be created as the top-level container in the simulation.

Be cautious, as changing the parent path of a container will result in different absolute path to the respective container and might break the formulas that use absolute paths for variables definition. You might have to adjust the absolute paths accordingly, by manually appending the parent path to the aliases.

### Neighborhoods
To allow replacement or addition of structures to the spatial structure (e.g., adding tumor, or replacing the kidney) within modules, the module has to come with the information about how the organ (or just some containers) are connected to the whole organism.

A new UI allows creation of neighborhoods without drag-and-drop. The user selects (or types in the full path to) the neighbors.

Exporting a container to PKML also exports all of its sub-containers and all neighborhoods that have this container or its sub-containers as a neighbor.

If an Extension module defines a neighborhood with a neighbor that is not present in the final model structure, the neighborhood is simply ignored

### Export of containers to pkml

 As described in ["Modularization concept"](../Modularization-concept.md), individual-specific parameters in PK-Sim modules are not stored directly in the spatial structure but in the Individual BB. When saving a container from a PK-Sim module to pkml, the user must select an Individual and, if applicable, the proteins for which the expression in this container should be allocated.

 When exporting the container, all neighborhoods associated to this container are also exported.

 ### Renaming containers

 When renaming a container, the software suggests changing the neighbor of all neighborhoods associated with the container to the new name.
