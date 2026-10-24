# Unified Multi-Crop Vision Transformer for West African Agricultural Disease Diagnosis

A shared Vision Transformer baseline for plant-disease classification across six selected crop groups and 29 labels. This repository documents the completed baseline, its measured results, the current limitation, and a proposed follow-up study.

## Project status

The baseline training and held-out test evaluation are complete. The test accuracy is **81.21%**, compared with **58.97%** for the classical feature-based baseline. The test set was drawn from a random, class-stratified split of the pooled data. These results do **not** establish generalization to unseen sources, farms, or seasons.

Multi-crop plant-disease classifiers and Vision Transformers already exist. The project therefore does not claim that a multi-crop ViT is new. Its defensible follow-up question is whether one model can maintain per-crop performance on this exact six-crop, 29-label collection when test data are grouped by source, farm, or season.

## Dataset

The cleaned collection contains 67,443 images across 29 labels. Taro was used as a data-availability substitute for Yam; results on Taro must not be described as Yam results. One corrupted artifact was removed before the final manifest was created. Images were resized to 224 x 224 and split by class-stratified random sampling (70/15/15, `random_state=42`): 47,210 train, 10,116 validation, and 10,117 test images.

| Crop group | Source | Classes | Images |
|---|---|---:|---:|
| Cassava | Kaggle | 5 | 21,397 |
| Maize | Kaggle and aggregated sources as documented in the EDA | 4 | 4,188 |
| Taro (substitute for Yam) | Mendeley Data | 5 | 21,220 |
| Tomato | PlantVillage | 10 | 16,011 |
| Pepper | PlantVillage | 2 | 2,475 |
| Potato | PlantVillage | 3 | 2,152 |
| **Total** |  | **29** | **67,443** |

## Models and results

The classical baseline uses 34 hand-crafted colour-histogram and GLCM texture features. The deep-learning baseline uses a pretrained ViT-Base/16 backbone with a custom classification head, fine-tuned for four epochs.

| Model | Train accuracy | Validation accuracy | Test accuracy |
|---|---:|---:|---:|
| Classical ML baseline | Not recorded | Not recorded | 58.97% |
| Custom ViT, epoch 4 | 88.13% | 81.36% | **81.21%** |

The ViT result is 22.24 percentage points above the classical baseline on the reported test split. Since the split is random and not source-held-out, do not present this as cross-source or field generalization.

## Proposed follow-up algorithm

The following is a proposal for the next study, not an implemented result:

1. Preserve crop, disease label, source or farm, season or acquisition batch, and image identity for each sample; check for duplicate and near-duplicate images.
2. Create leakage-resistant train, validation, and test splits that hold out entire sources, farms, or acquisition groups. Confirm that the held-out groups contain enough of the labels being evaluated.
3. Use a shared image encoder with a crop-prediction head and a crop-conditioned disease head. Train with weighted crop and disease cross-entropy losses; estimate class weights from training data and tune loss weights on validation data only.
4. Report per-crop and per-class macro-F1, recall, balanced accuracy, confusion matrices, calibration, model size, latency, and memory. Set any uncertainty threshold using validation data and refer low-confidence cases for human review.

This proposal depends on reliable group metadata. If the current images do not include source, farm, season, or acquisition-group identifiers, the team must recover or collect them before claiming a grouped generalization test. Victor's review is a working analysis for him to verify and defend with the group and lecturer.

## Team

| Member | Contribution / assigned role |
|---|---|
| Makinde Ayomide Daniel (2023/12127) | Team lead and implementation lead |
| Sanni Samiat Ajoke | Literature reviewer |
| Sodamade Ismail | Writing coordinator |
| Nweze Sharon | Visualisation specialist |
| Shittu Qais Ayomide | Documentation support |
| Izundu Victor Chidera (2023/12156) | Literature evidence and evaluation lead; assigned ten-paper review and research-gap synthesis |

## Repository contents

- `notebooks/ML_DL_Universal_Crop_Baseline.ipynb` - data preparation, classical baseline, ViT training, and evaluation.
- `docs/Full_Manuscript.docx` - current manuscript draft.
- `docs/ICT324_Project_Summary.docx` - current project status and remaining work.
- `docs/EDA_Report_UniPlant_WA.docx` - data acquisition, validation, and exploratory analysis.
- `docs/Results_Section.docx` - final aggregate and per-class evaluation results.
- `docs/Verified_Reference_List.docx` - reconciled working bibliography: 50 active sources. Item 51 is the added methodology reference; historical numbering gaps are documented.
- `docs/Victor_10_Paper_Review.docx` - working review of ten core papers and supporting comparisons, prepared for Victor's verification.
- `results/test_evaluation_report.txt` - final classification report across all 29 labels.

## Reproducing the baseline

1. Open the notebook in Google Colab.
2. Mount Google Drive and update the data paths to match your Drive folders.
3. Run the cells in order. The original dataset and trained checkpoints are not included in this repository.

## Tools

Google Colab with a T4 GPU, PyTorch, timm, scikit-learn, OpenCV, scikit-image, and pandas.

## License and reuse

This repository is an academic project and does not include an open-source license. Contact the team before reusing project materials.
