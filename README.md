# Unified Multi-Crop Vision Transformer for West African Agricultural Disease Diagnosis

A Vision Transformer approach to cross-crop plant disease classification across six West African staple crops — developed for ICT 324, Bells University of Technology.

## Project Overview

Existing plant disease detection models are overwhelmingly single-crop specialists, despite West African smallholder farms typically cultivating multiple crops together. This project builds a **single Vision Transformer** capable of diagnosing disease across **six economically important crops** — Cassava, Maize, Taro (substituting Yam), Tomato, Pepper, and Potato — rather than requiring six separate specialist models.

**Final Results:** 81.21% test accuracy (Custom ViT) vs. 58.97% (Classical ML baseline) — a 22.24 percentage-point improvement.

## Team

| Role | Member |
|---|---|
| Team Lead & Implementation Lead | Makinde Ayomide Daniel (2023/12127) |
| Literature Reviewer | Sanni Samiat Ajoke |
| Writing Coordinator | Sodamade Ismail |
| Visualisation Specialist | Nweze Sharon |
| Documentation Support | Shittu Qais Ayomide |

Department of Computer Science, Bells University of Technology, Ota.

## Repository Structure

```
├── notebooks/
│   └── ML_DL_Universal_Crop_Baseline.ipynb   # Full pipeline: data prep, classical baseline, ViT training, evaluation
├── docs/
│   ├── Full_Manuscript.docx                  # Complete report: Intro, Lit Review, Methodology, Results, Discussion, Conclusion, References
│   ├── ICT324_Project_Summary.docx           # Condensed project summary
│   ├── EDA_Report_UniPlant_WA.docx           # Exploratory data analysis report
│   ├── Results_Section.docx                  # Standalone Results section with per-class breakdown
│   └── Verified_Reference_List (1).docx      # 50-paper literature list, DOI-verified
├── results/
│   └── test_evaluation_report.txt            # Final test-set classification report (all 29 classes)
└── README.md
```

## Dataset

67,443 images across six crops, sourced from Kaggle (Cassava, Maize), Mendeley Data (Taro — substituting Yam due to documented data scarcity), and PlantVillage (Tomato, Pepper, Potato). Full composition and justification for the Taro substitution are detailed in `docs/Full_Manuscript.docx` (Sections 1.3 and 3.1).

| Crop | Source | Classes | Images |
|---|---|---|---|
| Cassava | Kaggle | 5 | 21,397 |
| Maize | Kaggle | 4 | 4,188 |
| Taro (substitute for Yam) | Mendeley Data | 5 | 21,220 |
| Tomato | PlantVillage | 10 | 16,011 |
| Pepper | PlantVillage | 2 | 2,475 |
| Potato | PlantVillage | 3 | 2,152 |
| **Total** | | **29** | **67,443** |

Data was split 70/15/15 (train/validation/test) using stratified sampling, and all images were resized to 224×224 for model input.

## Methodology

**Classical ML Baseline** — Colour histograms and GLCM texture features (34 features/image) fed into a classical classifier. Test accuracy: **58.97%**.

**Custom Vision Transformer** — ViT-Base (patch16, 224×224) backbone, pretrained and fine-tuned with a custom classification head (LayerNorm → Linear → GELU → Dropout → Linear). Trained 4 epochs with checkpointed training to accommodate free-tier GPU session limits.

## Results

| Model | Train Acc. | Validation Acc. | Test Acc. |
|---|---|---|---|
| Classical ML Baseline | — | — | 58.97% |
| Custom ViT (Epoch 4, final) | 88.13% | 81.36% | **81.21%** |

Per-class performance varied meaningfully: Tomato, Pepper, and Potato classes achieved F1-scores of 0.93–1.00, while underrepresented Taro disease stages (Late_Blight, Mid_Blight) performed considerably weaker — a finding directly tied to the class imbalance and data scarcity discussed in the manuscript's Discussion section. Full per-class metrics are in `results/test_evaluation_report.txt`.

## Reproducing This Work

1. Open `notebooks/ML_DL_Universal_Crop_Baseline.ipynb` in Google Colab
2. Mount Google Drive and ensure dataset paths match your own Drive structure (see notebook markdown cells for expected paths)
3. Run cells sequentially: data acquisition → preprocessing → classical baseline → ViT training → evaluation

**Note:** Trained model checkpoints (`.pt` files) are not included in this repository due to file size (~1GB each) — retrain using the notebook, or contact the team for checkpoint access.

## Tools

Google Colab (T4 GPU), PyTorch, timm, scikit-learn, OpenCV, scikit-image, pandas.

## License

Academic project submitted for ICT 324, Bells University of Technology. Not licensed for commercial use.
