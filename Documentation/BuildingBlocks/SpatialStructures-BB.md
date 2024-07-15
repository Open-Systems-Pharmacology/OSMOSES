## Spatial Structures (SS) Building Block (BB)

### Parent path
The spatial structures (SS) of the models are organized in a hierarchy of containers, where each container can be a *child* of a *parent* container. At the same time, `Container A` can be a *child* of exactly one *parent container* `Container B` and the *parent* of multiple containers. Each container has a property `Parent path` which specifies the **full path** to its parent container.

In a module, the user can change the parent container property for the top level containers. When combining modules, the containers (and all their children) of the module will be placed under the container specified in the `Parent path` property. If the parent path of `Container A` is empty OR the container with the specified parent path is not found in the simulation, `Container A` will be created as the top-level container in the simulation.

Be cautios, as changing the parent path of a container will result in different absolute path to the repsective container and might break equations tha use absolute paths for variables definition. You might have to adjust the absolute paths accordingly, by manually appending the parent path to the alisases.

### Neighborhoods
To allow replacement or addition of structures to the spatial structure (e.g., adding tumor, or replacing the kidney) within modules, the module has to come with the information about how the organ (or just some containers) are connected to the whole organism.

A new UI allows creation of neighborhoods without drag-and-drop. The user selects (or types in the full path to) the neighbors.

Exporting of a container to a PKML also exports all its sub-containers and all neighborhoods that have this container or his sub-containers as a neighbor.

If an extension module defines a neighborhood with a neighbor that is not present in the model configuration, the model configuration is created and warnings are displayed ("WARNING: The neighborhood "Tumor_pls_Head_pls" was not created, as the neighbor "Organism|Head|Plasma" is not present!"). **2DO** check actual implementation.

### Parameters

 Parameters that are defined in the Individuals-BB be populated with "NaN"-values in the SS-BB of PK-Sim modules. **2DO** https://github.com/Open-Systems-Pharmacology/MoBi/issues/1401