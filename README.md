# Cybercrime Detection with K-Nearest Neighbors

This repository contains a notebook project that classifies IoT network traffic into the attack labels in the RT-IoT2022 dataset using a K-Nearest Neighbors (KNN) classifier.

## Project files

| File | What it contains |
|---|---|
| [`project/AssignmentML.ipynb`](project/AssignmentML.ipynb) | Main, executable Jupyter notebook. It loads and explores the CSV, encodes the target, scales and one-hot encodes features, trains KNN, and reports test and cross-validation metrics. It also includes explanatory markdown and saved outputs. |
| [`project/AssignmentML.html`](project/AssignmentML.html) | Browser-friendly export of the notebook, including its saved results. Use this to read the work without launching Jupyter. |
| [`researchpaper/Wireless Communications and Mobile Computing - 2022 - Veena - C SVM Classification and KNN Techniques for Cyber Crime.pdf`](researchpaper/Wireless%20Communications%20and%20Mobile%20Computing%20-%202022%20-%20Veena%20-%20C%20SVM%20Classification%20and%20KNN%20Techniques%20for%20Cyber%20Crime.pdf) | Background/reference article on SVM and KNN techniques for cybercrime classification. |
| [`ppt and IEE/KNN_CyberCrime_Presentation.pptx`](ppt%20and%20IEE/KNN_CyberCrime_Presentation.pptx) | Slide presentation describing the cybercrime KNN project. |
| [`ppt and IEE/ieee_paper_with_results119.pdf`](ppt%20and%20IEE/ieee_paper_with_results119.pdf) | Project paper/report with results. |

The personal résumé directory is intentionally excluded from this repository.

## Run the notebook

1. Install Python 3 and Jupyter, then install the packages listed in [`requirements.txt`](requirements.txt).
2. Obtain the RT-IoT2022 data and put `RT_IOT2022.csv` next to `AssignmentML.ipynb` in the `project/` directory. The dataset is not included here.
3. Open `project/AssignmentML.ipynb` in Jupyter and run the cells from top to bottom.

The notebook expects a CSV containing an `Attack_type` target column and an `Unnamed: 0` index column. It drops both from the feature matrix and uses `Attack_type` as the classification label.

## Notebook walkthrough

1. **Load and inspect data (cells 0–12):** read the CSV, show sample rows, dimensions, column types and summary statistics, and count missing values.
2. **Prepare target and features (cells 13–25):** remove the target and index from `X`; encode attack names into integer classes; standardize numeric inputs and one-hot encode categorical inputs.
3. **Train and evaluate (cells 26–41):** create a stratified 80/20 train/test split, train KNN with `k=5`, calculate test accuracy, display a confusion matrix, and run five-fold cross-validation.
4. **Choose `k` and report results (cells 42–56):** compare values 1–15 using cross-validation, plot the scores, show a confusion matrix and print precision, recall and F1 by class.

Some operations are repeated in later cells: target encoding appears twice, preprocessing appears twice, and the same plots are generated more than once. These repetitions make the notebook longer but do not add new analysis.

## Results saved in the notebook

The notebook's saved outputs show 12,399 rows, 83 input columns before preprocessing, and 92 columns after encoding. The `k=5` model reports 0.9940 test accuracy and a five-fold mean accuracy of 0.9965. The separate search over `k=1…15` selects `k=2` with mean accuracy 0.9817. In the saved classification report, the `Wipro_bulb` class has 29 test examples and lower recall (0.62) than the other two classes.

These are the results recorded in the notebook, not a freshly reproduced run. The notebook currently fits scaling and one-hot encoding on the full dataset before making the holdout split; cross-validation also uses already transformed full-dataset features. This can leak information from validation/test folds into preprocessing and make scores optimistic. For a stronger evaluation, put the preprocessing transformer and KNN estimator in a scikit-learn `Pipeline`, and cross-validate the full pipeline. The search over `k` also uses a different CV setup from the standalone five-fold result, so compare those figures with care.

