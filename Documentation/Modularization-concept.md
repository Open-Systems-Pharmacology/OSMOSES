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
- [*Individual*](BuildingBlocks/Individuals-BB.md): Parameter set describing the physiological properties of an individual. The parameter set referred to is limited to the parameters provided by PK-Sim. Technically comparable to the Parameter Values BB with additional metadata.
- [*Expression profile*](BuildingBlocks/ExpressionProfile.md): Parameter set describing the expression of a protein.
