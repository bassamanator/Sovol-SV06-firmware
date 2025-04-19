# Converting Prusa Slicer config bundle to Orca Slicer

Orca Slicer configs can be converted from Prusa Slicer using https://github.com/apparle/SuperSlicer_to_Orca_scripts . It is a fork of theophile's script with some enhancements ([theophile/SuperSlicer_to_Orca_scripts#43](https://github.com/theophile/SuperSlicer_to_Orca_scripts/pull/43) ) which haven't been merged yet. It does require a few perl modules which can be installed using cpanminus (or your favorite perl module manager).

Most of the script is automated, with the exception of `printer_model` field that must be manually specified with a json file. This attribute is used by OrcaSlicer to load the appropriate build plate images & STLs.

```
perl SuperSlicer_to_Orca_scripts/superslicer_to_orca.pl --input PrusaSlicer_config_bundle-2.9.2.ini --nozzle-size 0.4 --compatible_printers_condition KEEP --skip-link-system-printer --printer-models-json printer_models_conversion_mapping.json --output-config-bundle OrcaSlicer_config_bundle-2.9.2-DEV.zip
```

## How to use OrcaSlicer bundle:
This config bundle can be imported into OrcaSlicer : `File > Import > Import configs...`

Note, after importing OrcaSlicer does reformat the json files and add some more default attributes, so the contents of this file cannot directly be compared with the contents of the zipped bundle.
