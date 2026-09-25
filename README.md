# CAC2026 Data Challenge

This repository contains the dataset and supporting materials for the **CAC2026 Data Challenge**, a comparative study of chemometric modelling strategies for olive oil quality assessment. The challenge was organised in the context of the 20th Chemometrics in Analytical Chemistry conference (CAC2026) in Tarragona, Catalonia, Spain.

The task is to classify olive oil samples according to the presence or absence of the **musty sensory defect** using complementary instrumental fingerprints. The reference classification was obtained by an official sensory panel.

## Dataset overview

The dataset contains measurements from three analytical platforms:

- **UV-Vis spectroscopy:** absorbance from 300 to 1000 nm, with 701 variables per measurement
- **ATR-FTIR spectroscopy:** absorbance from approximately 3230 to 673 cm^-1, with 549 variables per measurement
- **Headspace mass spectrometry (HS-MS):** ion-count signals from m/z 50 to 350, with 301 variables per measurement

The calibration set contains 220 olive oil samples:

- 144 samples without a detected musty defect (`0`)
- 76 samples with a detected musty defect (`1`)

An external test set contains 24 additional samples. The repository includes the test labels to support retrospective comparison and methodological research after completion of the original blind challenge.

## Replicate structure

Instrumental measurements were collected as analytical replicates:

| Data block | Calibration measurements | Test measurements | Replicates per sample |
| --- | ---: | ---: | ---: |
| UV-Vis | 440 | 48 | 2 |
| ATR-FTIR | 660 | 72 | 3 |
| HS-MS | 440 | 48 | 2 |

The class label is defined at the **sample level**, not the replicate level. All replicates belonging to one sample must remain together during calibration, cross-validation, and testing. Splitting replicates from the same sample between training and validation sets causes information leakage and can produce overly optimistic performance estimates.

## Workbook contents

The file `CAC2026_Data_challenge_Datasets.xlsx` contains nine worksheets:

| Worksheet | Contents |
| --- | --- |
| `CAL labels` | Musty class labels for the 220 calibration samples |
| `CAL metadata` | Olive-oil grade, fruity intensity, bitterness, and pungency for the calibration samples |
| `CAL UV-Vis` | UV-Vis calibration measurements |
| `CAL ATR-FTIR` | ATR-FTIR calibration measurements |
| `CAL HS-MS` | HS-MS calibration measurements |
| `TEST labels` | Musty class labels for the 24 external test samples |
| `TEST UV-Vis` | UV-Vis test measurements |
| `TEST ATR-FTIR` | ATR-FTIR test measurements |
| `TEST HS-MS` | HS-MS test measurements |

The first column of each instrumental worksheet identifies the sample. The remaining columns contain the wavelength, wavenumber, or mass-channel variables.

## Repository contents

- `CAC2026_Data_challenge_Datasets.xlsx`: calibration and test data, labels, and calibration metadata
- `DataChallengeDescription.pdf`: task description and original participation conditions
- `ParticipationForm`: methodological questionnaire supplied to participants
- `ResponsesToForm`: anonymised responses describing the submitted modelling workflows
- `BorrasOliveOilDefectsDataFusion.pdf`: reference article describing the source dataset and earlier data-fusion study

## Challenge objective

Participants were asked to construct a model that predicts the musty class of every test sample. They were free to use one, two, or all three instrumental blocks and to select any preprocessing, variable-selection, data-fusion, classification, and validation strategy.

The primary evaluation measure was the F1 score for the musty class:

```text
F1 = 2 * (precision * recall) / (precision + recall)
```

Model parsimony was used as a secondary criterion. Although the activity was called a data challenge, its main scientific purpose was to compare how analysts make decisions about replicate handling, preprocessing, outlier detection, block selection, data fusion, model construction, optimisation, and validation.

## Study context

The associated comparative study examined 31 submitted workflows representing 48 declared contributors, including one workflow generated using ChatGPT. The submissions covered partial least squares methods, support vector machines, tree-based methods, neural networks, and other approaches.

The study found substantial methodological diversity. Workflows combining complementary instrumental blocks generally performed better than single-block approaches, while greater model complexity did not consistently improve external performance. The results emphasised sample-level validation, parsimonious modelling, and transparent reporting of the complete analytical workflow.

## Recommended use

1. Load the calibration labels, metadata, and one or more instrumental blocks.
2. Match observations using the sample identifier in the first column.
3. Average replicates or use a grouped modelling strategy that preserves sample identity.
4. Apply preprocessing parameters using calibration data only.
5. Perform cross-validation with all replicates from a sample assigned to the same fold.
6. Evaluate final predictions against `TEST labels` using the F1 score and complementary classification metrics.

## Citation

When using this repository, please cite the associated manuscript:

> Ezenarro, J., Schorn-García, D., Vera-i-Valls, N., Pellegrino, M., Busto, O., Mestres, M., Aceña, L., Capdevila, J., Ruisánchez, I., Riu, J., Ferré, J., & Boqué, R. (2026). The CAC2026 Data Challenge: A comparative study of chemometric modelling strategies for olive oil quality assessment. *Manuscript submitted to Analytica Chimica Acta*

May also be of interest, the study that introduced the source dataset:

> Borràs, E., Ferré, J., Boqué, R., Mestres, M., Aceña, L., Calvo, A., & Busto, O. (2016). Olive oil sensory defects classification with data fusion of instrumental techniques and multivariate analysis (PLS-DA). *Food Chemistry, 203*, 314-322. https://doi.org/10.1016/j.foodchem.2016.02.038

## License

The dataset and original repository materials are licensed under the [Creative Commons Attribution 4.0 International License](LICENSE.txt). Reuse and adaptation are permitted for any purpose, provided appropriate attribution is given and changes are indicated.

The included publisher-formatted reference article is a separate published work and is not covered by this repository license. Its publisher copyright and reuse terms continue to apply.
