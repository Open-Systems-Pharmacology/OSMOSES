## Where to get PK-Sim and MoBi version 12

OSMOSES (version 12 of the OSPS) is currently under development and has not entered the beta testing phase yet. However, a curious user can download the portable versions of PK-Sim (https://ci.appveyor.com/project/open-systems-pharmacology-ci/pk-sim/branch/develop/artifacts) and MoBi (https://ci.appveyor.com/project/open-systems-pharmacology-ci/mobi/branch/develop/artifacts) to test the new concept. Any feedback will be greatly appreciated. Contributions to this documentation are equally important as reporting bugs (for MoBi: https://github.com/Open-Systems-Pharmacology/MoBi/issues; for PK-Sim: https://github.com/Open-Systems-Pharmacology/PK-Sim/issues) or creating documentation requests/questions (https://github.com/Open-Systems-Pharmacology/OSMOSES/issues).

## How does OSMOSES work?

The concept is described in the article ["OSMOSES modularization concept"](Modularization-concept.md).

## Project conversion

To get the full advantage of the new modularization concept with the support of individuals and expression profiles in MoBi, you will require a PBPK model created with PK-Sim V12. For PK-Sim project created with previous version, the project must be re-created from a snapshot. Open your project in PK-Sim V12, export it to snapshot, and then re-load from snapshot (see [documentation](https://docs.open-systems-pharmacology.org/working-with-pk-sim/pk-sim-documentation/importing-exporting-project-data-models#exporting-project-to-snapshot-loading-project-from-snapshot)). After this, all simulations in the project will have the new structure and sending them to MoBi will properly transfer the individuals and expression profiles.

2DO https://github.com/Open-Systems-Pharmacology/OSMOSES/issues/59