## Converting Prusa Slicer config bundle to Orca Slicer

Orca Slicer configs can be converted from Prusa Slicer using https://github.com/apparle/SuperSlicer_to_Orca_scripts . It is a fork of theophile's script with some enhancements ([theophile/SuperSlicer_to_Orca_scripts#43](https://github.com/theophile/SuperSlicer_to_Orca_scripts/pull/43) ) which haven't been merged yet. It does require a few perl modules which can be installed using cpanminus (or your favorite perl module manager).

Most of the script is automated, with the exception of `printer_model` field that must be manually specified with a json file. This attribute is used by OrcaSlicer to load the appropriate build plate images & STLs.

```
perl SuperSlicer_to_Orca_scripts/superslicer_to_orca.pl --input PrusaSlicer_config_bundle-2.9.2.ini --nozzle-size 0.4 --compatible_printers_condition KEEP --skip-link-system-printer --printer-models-json printer_models_conversion_mapping.json --output-config-bundle OrcaSlicer_config_bundle-2.9.2-DEV.zip
```

## How to use OrcaSlicer bundle:
This config bundle can be imported into OrcaSlicer : `File > Import > Import configs...`

Note, OrcaSlicer reformats the json files and adds some default attributes, so the imported files cannot be directly compared with the contents of the zipped bundle. But you can use json comparison tools like [https://www.jsondiff.com/](https://www.jsondiff.com/) to analyze the differences if needed.

