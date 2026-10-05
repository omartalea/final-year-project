# Diabetic Retinopathy: Blur Robustness Study

**Final year research project.** It addresses a research gap: we know little about how deep-learning classifiers for diabetic retinopathy (DR) behave when retinal images lose quality.

Most DR studies report accuracy on clean images only. This study asks a different question: **as retinal fundus images become progressively blurred, do the models' predictions and visual attention degrade in a consistent way?**

📄 **[Read the paper (IEEE format)](Omar_Alghamdi_IEEE.pdf)**  
📊 **[View the experiment results notebook](DR_Blur_Robustness_Study.ipynb)** (outputs only; the code is private)

## Method
- **Data:** retinal fundus images graded into 5 DR classes (No DR to Proliferative DR), with train, validation and test splits and a patient-leakage check
- **Controlled degradation:** 5 blur levels, from **B0** (clean) through B1 to **B4** (strong Gaussian blur), with the blur confirmed by a Laplacian-variance sharpness check
- **Models:** a CNN trained from scratch, plus **ResNet50**, **DenseNet201** and **EfficientNetB0** using ImageNet transfer learning
- **Evaluation:** Macro F1, Quadratic Weighted Kappa, ROC-AUC, confusion matrices, per-class sensitivity, and the direction of grading errors
- **Statistics:** one-way ANOVA with Tukey HSD post hoc tests across models and blur levels
- **Explainability:** Grad-CAM heatmaps across B0 to B4, to check whether model attention stays on retinal structures

## Key results
| Model | Macro F1 (B0) | QWK (B0) | Macro AUC (B0) | F1 retained at B4 |
|---|---|---|---|---|
| ResNet50 | **0.495** | 0.680 | **0.811** | 0.169 |
| EfficientNetB0 | 0.472 | **0.688** | 0.805 | 0.449 |
| DenseNet201 | 0.456 | 0.686 | 0.809 | 0.365 |
| Simple CNN | 0.221 | 0.208 | 0.602 | **0.749** |

The transfer-learning models score best on clean images but lose most of their performance under strong blur. ResNet50 keeps only ~17% of its F1 at B4. So clean-image accuracy alone is a poor guide to reliability on real, lower-quality retinal images.

## Data and attribution
The experiments used the unified EyePACS + APTOS + Messidor diabetic retinopathy collection published on Kaggle ([ascanipek/eyepacs-aptos-messidor-diabetic-retinopathy](https://www.kaggle.com/datasets/ascanipek/eyepacs-aptos-messidor-diabetic-retinopathy)), drawn from the original EyePACS, APTOS 2019 and Messidor datasets. A few de-identified fundus images appear only as illustrative figures in the paper and the notebook. No dataset files or trained weights are included in this repository.

## Tech
Python · TensorFlow / Keras · scikit-learn · OpenCV · statsmodels · Grad-CAM · Kaggle GPU
