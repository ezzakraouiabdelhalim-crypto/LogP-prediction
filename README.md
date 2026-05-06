# LogP Prediction

Predicting octanol/water partition coefficient (LogP) from molecular structure using a stacking ensemble of tuned tree-based models.

## Results

| Model | RMSE | R² |
|-------|------|-----|
| LightGBM | 0.755 | 0.607 |
| XGBoost | 0.751 | 0.610 |
| **Stack (LGBM + XGB)** | **0.726** | **0.636** |

## Features

- **Dataset:** ~4,200 molecules from MoleculeNet Lipophilicity (ChEMBL)
- **Molecular features:** 175+ RDKit descriptors + 2048-bit Morgan fingerprints
- **Data cleaning:** SMILES standardization, fragment removal, uncharging, atom filtering
- **Scaffold split** for realistic evaluation
- **Hyperparameter tuning** with Optuna (50 trials per model)
- **Applicability domain** check using Tanimoto similarity
- **SHAP analysis** for model interpretability

## Project Structure

```
├── LogP_final.ipynb    # Full pipeline
└── README.md
```

## How It Works

1. **Input:** SMILES string
2. **Feature extraction:** RDKit descriptors + Morgan fingerprints
3. **Prediction:** Stacking ensemble (LGBM + XGB → Ridge meta-learner)
4. **Output:** Predicted LogP + AD check

## Example Predictions

| Molecule | Predicted | Real LogP | Error |
|----------|-----------|-----------|-------|
| Paracetamol | 0.46 | 0.46 | 0.00 |
| Diazepam | 2.76 | 2.82 | 0.06 |
| Caffeine | -0.18 | -0.07 | 0.11 |
| Naphthalene | 2.93 | 3.30 | 0.37 |
| Ethanol | -0.48 | -0.31 | 0.17 |

## Notes

- MolLogP (RDKit's built-in LogP estimate) dominates feature importance — the model essentially learns to correct RDKit's calculator using other molecular descriptors
- LogP prediction is inherently harder than solubility due to stronger dependence on 3D conformations and solvation effects that 2D descriptors can't fully capture
- Model struggles with highly lipophilic molecules (LogP > 4) due to limited training examples in that range

## Tools

Python, pandas, scikit-learn, XGBoost, LightGBM, RDKit, Optuna, SHAP
