## Project conversion

To get the full advantage of the new modularization concept with the support of individuals and expression profiles in MoBi, you will require a PBPK model created with PK-Sim V12. For PK-Sim project created with previous version, the project must be re-created from a snapshot. Open your project in PK-Sim V12, export it to snapshot, and then re-load from snapshot (see [documentation](https://docs.open-systems-pharmacology.org/working-with-pk-sim/pk-sim-documentation/importing-exporting-project-data-models#exporting-project-to-snapshot-loading-project-from-snapshot)). After this, all simulations in the project will have the new structure and sending them to MoBi will properly transfer the individuals and expression profiles.

## Known issues limitations

- Having more than one `VenousBlood` organ in the structure (e.g., when adding an offspring structure with a separate `VenousBlood`), some administration protocols (intravenous infusion) will be applied to both organs, as the `TARGET` of the infusion process is set to `Plasma` and `VenousBlood` (https://github.com/Open-Systems-Pharmacology/MoBi/issues/1406). As a workaround, both organs should have distinct names (e.g., `VenousBlood_offspring`), or the tag `Organism` should be added to the container criteria of the transport process.
