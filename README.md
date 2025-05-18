# CodeBERT-COSMIC-FSM: Graduation Thesis - Automated Software Size Estimation from Source Code

This repository contains the implementation, experiments, and report for the **graduation thesis** submitted in partial fulfillment of the requirements for the Bachelor's degree in Computer Engineering at Boğaziçi University. The project uses a pretrained CodeBERT model to estimate software size metrics (COSMIC Functional Size Measurement) directly from Python source code, offering an automated alternative to traditional requirement-document-based estimation.

---

## 📂 Repository Structure

```
├── codebert_cosmic_fsm.py      # Main training & evaluation script
├── codesearchnet_python.csv    # Raw dataset (first 2,000 samples from CodeSearchNet)
├── training_stats_without_comments.csv  
├── codebert_best_model_without_comments.pt
└── results/
    └── predictions_<N>_without_comments.csv
```

---

## 🚀 Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/your-username/codebert-cosmic-fsm.git
cd codebert-cosmic-fsm
```

### 2. Create a virtual environment (recommended)
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install torch torchvision torchaudio \
            transformers \
            pandas \
            scikit-learn \
            nltk \
            matplotlib
```
> **Note:** You must download NLTK stopwords once:
> ```python
> >>> import nltk
> >>> nltk.download('stopwords')
> ```

---

## 🗄 Dataset

- **codesearchnet_python.csv**  
  - A subset (2,000 functions) of the CodeSearchNet Python dataset  
  - Columns include:
    - `Code` (raw source code)
    - `E, W, R, X, CFP` (COSMIC size metrics: Entry, Write, Read, eXit, and Combined Functional Points)

Place `codesearchnet_python.csv` in the project root before running.

---

## ⚙️ Preprocessing

The script automatically:

1. Reads the first 2,000 rows of `codesearchnet_python.csv`.
2. Filters out functions with language code “C”.
3. Removes comments and docstrings (`remove_comments_and_docstrings`).
4. Splits into **train** (80%) and **test** (20%) sets.
5. Creates a 90/10 train/validation split for model training.

---

## 🏋️ Training

Run the main script to train and validate the regression model:

```bash
python codebert_cosmic_fsm.py
```

Key hyperparameters:

- **Model**: `microsoft/codebert-base` (Roberta architecture)
- **Batch size**: 16
- **Epochs**: 5
- **Learning rate**: 2e-5
- **Loss**: MAE (L1)

Outputs:

- `training_stats_without_comments.csv` — per-epoch loss & metrics  
- `codebert_best_model_without_comments.pt` — best model weights  

---

## 🔍 Evaluation & Testing

After training, the script:

1. Loads the best checkpoint.
2. Tokenizes the test set (max length 256).
3. Predicts the five COSMIC metrics.
4. Saves results to `results/predictions_<N>_without_comments.csv`.
5. Computes global and per-dimension MAE, RMSE, NMAE, and accuracy (±0.5 tolerance).
6. Plots scatter figures of true vs. predicted values.

---

## 📈 Results

- **Global Test MAE / RMSE**  
- **Per-dimension metrics** (E, W, R, X, CFP)  
- **Accuracy** within a ±0.5 tolerance  

Refer to the printed logs and generated CSV for detailed numbers and visualizations.

---

## 🛠 Customization

- **Comments Inclusion**  
  - Toggle `REMOVE_COMMENTS = False` at top to keep inline comments.
  - Filenames and postfixes will update to `with_comments`.

- **Max Sequence Length**  
  - Adjust `MAX_LEN` (default 512) for longer/shorter context windows.

- **Batch Size & Epochs**  
  - Modify `batch_size` and `epochs` to suit your hardware constraints.

---

## 🤝 Contributing

1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/xyz`)  
3. Commit your changes (`git commit -m "Add xyz"`)  
4. Push to your fork (`git push origin feature/xyz`)  
5. Open a Pull Request  

---

## 📜 Thesis & License

This thesis is submitted in partial fulfillment of the requirements for the Bachelor's degree in Computer Engineering at Boğaziçi University. The code and accompanying materials are released for academic and research purposes under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)** license. For commercial use or other permissions, please contact the author.

---

## ✉️ Contact

For questions or collaboration, please open an issue or contact **Your Name** at **your.email@example.com**.
