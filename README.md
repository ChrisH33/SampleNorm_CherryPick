# Sample Normalisation & Cherry Picking

A general-purpose Hamilton STAR liquid handling method for automated sample normalisation and cherry picking.

## Overview

This method reads a user-provided CSV worklist and performs automated pipetting transfers on a Hamilton ML STAR instrument. It supports flexible source-to-destination mappings with configurable sample and diluent volumes, enabling use cases such as plate copying, sample normalisation, and cherry picking across multiple plates.

## Repository Structure

```
├── Custom Dialogs/                         # Custom dialogs used within the method
├── Example Worklists/                      # Example & test worklists
├── Labware/                                # Labware used within the method
├── SampleNormalisation_CherryPicking.hsl   # Hamilton HSL method file
├── SampleNormalisation_CherryPicking.med   # Hamilton GUI Method
└── SampleNormalisation_CherryPicking.lay   # Main Hamilton layout
```

## Worklist Format

Worklists must be provided as `.csv` files with the following columns:

|Column|Description|
|---|---|
|`Source Label`|Barcode or label of the source labware|
|`Source Position`|Well position on the source plate (e.g. `A1`, `B3`)|
|`Destination Label`|Barcode or label of the destination labware|
|`Destination Position`|Well position on the destination plate|
|`Sample Volume`|Volume of sample to transfer (µL)|
|`Diluent Volume`|Volume of diluent to add (µL); use `0` if not required|

See the `Example Worklists/` folder for reference files covering a range of use cases.

## Method Workflow

1. **User Input** — The operator selects a worklist CSV via the Worklist Selection dialog.
2. **Worklist Handling** — The worklist is parsed and validated. The loop allows re-selection if the file is invalid.
3. **Load Instructions** — If not in simulation mode, the operator is guided to load labware onto the deck.
4. ~~**Barcode Scanning** — Deck barcodes are scanned and matched against the worklist.~~ `Not currently implemented`
5. **Sequence Creation** — Pipetting sequences are built from the validated worklist data.
6. **Pipetting** — Transfers are executed using the appropriate tip size and liquid class for each volume, diluent first.
7. **Completion** — A Slack notification is sent on completion and a summary dialog is displayed.

## Tip Sizes & Liquid Classes

|Tip|Liquid Class|
|---|---|
|10 µL filtered|`LowVolumeFilter_96COREHead_Water_DispenseSurface_Empty`|
|50 µL filtered|`10X_50F_RSB_JetEmpty`|
|300 µL filtered|`StandardVolumeFilter_96COREHead_Water_DispenseSurface_Empty`|
|1000 µL filtered|`HighVolumeFilter_Water_DispenseJet_Empty`|

## Requirements

- Hamilton ML STAR liquid handling system
- Hamilton Method Editor / Venus software
- Worklist CSV prepared according to the format above
