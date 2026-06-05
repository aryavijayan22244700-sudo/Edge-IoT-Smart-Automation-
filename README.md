# 🔌 Edge-IoT-Smart-Automation

> A hands-on learning journey through Edge Computing, IoT systems, and Smart Automation — from development environment setup to AI/ML integration.

---

## 📋 Table of Contents

- [About the Project](#about-the-project)
- [Learning Progress](#learning-progress)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)

---

## About the Project

This repository documents a structured, day-by-day progression through the foundations of Edge IoT and Smart Automation — covering hardware tooling, firmware development, version control, and machine learning concepts applied to embedded/edge systems.

---

## Learning Progress

### Day 1 — Version Control & GitHub Basics
- Installed **Git** and configured the local environment
- Learned GitHub fundamentals: repositories, commits, push/pull
- Set up GitHub account and connected it with the local Git installation

---

### Day 2 — Portfolio Deployment
- Built and deployed a **personal portfolio website**
- Learned the deployment workflow using GitHub Pages / hosting platform
- Practiced end-to-end: code → commit → deploy → live site

---

### Day 3 — Hardware & Firmware Tooling Setup
- Installed **KiCad** — open-source EDA tool for PCB schematic design
- Installed and completed setup of **PlatformIO IDE** (VSCode Extension)
  - Configured for embedded development workflow
  - Verified board support and build toolchain

---

### Day 4 — Applied Machine Learning: Loan Prediction Classifier

Completed a hands-on workshop on Applied ML — built and trained a **Decision Tree Classifier** in Google Colab to predict loan approval outcomes.

#### 🧠 Core Concepts Learned

**How Decision Trees Work**
- Instead of writing explicit formulas, the model asks a series of yes/no questions to divide data into clean groups
- **Root Node** — the very first question the tree asks to split the data
- **Leaf Nodes** — the final endpoints that provide the prediction (e.g. Approved / Denied)

**Entropy & Node Purity**
- **High Entropy** = a 50/50 mix of outcomes → the model is uncertain
- **Zero Entropy** = a group with only one outcome type → the model is fully certain
- The tree always picks the question that creates the **cleanest, most sorted groups** (maximum information gain)

---

#### 🛠️ Workshop Pipeline

```
Load & Clean → Encode & Split → Train & Plot → Interactive Test
```

**Step 1 — Import & Load Data**
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier

url = "https://raw.githubusercontent.com/shrikant-temburwar/Loan-Prediction-Dataset/master/train.csv"
df = pd.read_csv(url)
df = df.dropna()
```

**Step 2 — Encode & Split (80% train / 20% test)**
```python
df['Gender'] = df['Gender'].map({'Male': 1, 'Female': 0})
df['Married'] = df['Married'].map({'Yes': 1, 'No': 0})
df['Education'] = df['Education'].map({'Graduate': 1, 'Not Graduate': 0})
df['Loan_Status'] = df['Loan_Status'].map({'Y': 1, 'N': 0})
# ... (other categorical encodings)

features = ['Gender', 'Married', 'Education', 'Self_Employed',
            'ApplicantIncome', 'LoanAmount', 'Credit_History', 'Property_Area']
X = df[features]
y = df['Loan_Status']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**Step 3 — Train & Evaluate**
```python
model = DecisionTreeClassifier(max_depth=4, random_state=42)
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)
print(f"📈 Test Accuracy: {accuracy * 100:.2f}%")
```

**Step 4 — Custom Prediction**
```python
my_custom_profile = [[1, 0, 1, 1, 6000, 150, 1.0, 1]]
my_custom_df = pd.DataFrame(my_custom_profile, columns=features)
prediction = model.predict(my_custom_df)
# Output: 🎉 AI Decision: Loan APPROVED!
```

**Optional — Visualize the Decision Tree**
```python
from sklearn.tree import plot_tree
import matplotlib.pyplot as plt

plt.figure(figsize=(20, 12))
plot_tree(model, feature_names=features,
          class_names=['Denied', 'Approved'],
          filled=True, rounded=True, fontsize=12)
plt.show()
```

---

#### 📌 Key Takeaways

| Concept | Insight |
|---------|---------|
| Decision Trees | Learn by splitting data with yes/no questions |
| Entropy | Measures disorder; lower = purer, better split |
| `train_test_split` | Keeps test data unseen to evaluate real performance |
| `max_depth` | Limits tree complexity to prevent overfitting |
| Categorical Encoding | Text labels must be mapped to numbers before training |
| Cell Execution Order | Jupyter notebooks must be run top-to-bottom (NameError otherwise) |

> 🔗 Workshop Reference: [Applied ML — Loan Prediction Classifier](https://edge-iot-intern.web.app/loan%20prediction.html)

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Git & GitHub | Version control and collaboration |
| KiCad | PCB / schematic design |
| PlatformIO (VSCode) | Embedded firmware development |
| Python / ML Frameworks | Machine learning and data processing |

---

## Getting Started

```bash
# Clone this repository
git clone https://github.com/<your-username>/Edge-IoT-Smart-Automation.git

# Navigate into the project
cd Edge-IoT-Smart-Automation
```

> PlatformIO projects can be opened directly in VSCode with the PlatformIO extension installed.

---

## 📌 Notes

This repository is actively updated as the program progresses. Each day's work is documented with key learnings and setup steps for reproducibility.

---

*Last updated: Day 4 — Applied ML: Loan Prediction Classifier*
