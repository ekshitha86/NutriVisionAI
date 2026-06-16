# 🍎 NutriVision AI

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-1.45-red?style=for-the-badge&logo=streamlit)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.9-orange?style=for-the-badge&logo=scikit-learn)
![Groq](https://img.shields.io/badge/Groq-Llama4-purple?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**A Multimodal Nutrition Intelligence Platform**

*Upload a food image → Get instant AI-powered nutritional analysis, health insights, and an interactive nutrition chatbot.*

</div>

---

## 📌 Overview

NutriVision AI is a full-stack AI-powered web application that combines **Computer Vision**, **Machine Learning**, and **Large Language Models** to deliver a complete nutrition analysis experience.

The system identifies food from an uploaded image using a trained **Random Forest classifier** with **HOG + HSV feature extraction**, fetches real nutritional data from a structured **CSV nutrition dataset**, and provides **health outcome analysis** and **interactive Q&A** through Groq's Llama 4 Scout model.

This project was developed as part of **Project 16: Multimodal Platform for NutritionVQA** (Category: Nutra).

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔍 **Food Recognition** | Trained Random Forest classifier on 9 food classes using HOG + HSV features |
| 📊 **Real Nutrition Data** | Values pulled from structured CSV dataset (not AI-generated) |
| 🎯 **Confidence Score** | Shows model prediction confidence (%) with color-coded indicator |
| 🔧 **Manual Correction** | Dropdown to override wrong predictions |
| ❤️ **Health Analysis** | Benefits, cautions, health rating, good-for / avoid-if groups |
| 💬 **Nutrition Chatbot** | Ask anything about the food — powered by Groq + Llama 4 Scout |
| 🕓 **Session History** | Sidebar history of all analyzed foods in the current session |
| 🎨 **Premium Dark UI** | Glassmorphism design with animated components |

---

## 🏗️ System Architecture

```
Image Upload (Streamlit)
        │
        ▼
┌─────────────────────────────┐
│   predict_food(image)       │  ← Local Trained Model
│   RandomForestClassifier    │
│   HOG + HSV Features        │
│   food_classifier.pkl       │
└──────────┬──────────────────┘
           │  Predicted Label + Confidence
           ▼
┌─────────────────────────────┐
│   lookup_nutrition(food)    │  ← CSV Dataset (Real Data)
│   FOOD-DATA-GROUP*.csv      │
│   5 CSVs · 2395 food items  │
└──────────┬──────────────────┘
           │  Nutritional Values
           ▼
┌─────────────────────────────┐
│   get_health_outcomes()     │  ← Groq API (Llama 4 Scout)
│   ask_vqa()                 │  ← Groq API (Text only)
└─────────────────────────────┘
           │
           ▼
     Streamlit UI Display
```

---

## 📂 Repository Structure

```
NutriVisionAI/
├── app.py                     # Main Streamlit application
├── train_food_classifier.py   # Model training script (HOG + HSV + RF)
├── train_model.py             # Alternative training script
├── requirements.txt           # Python dependencies
├── .gitignore                 # Git exclusions
├── README.md                  # This file
│
├── models/
│   ├── food_classifier.pkl    # Trained RandomForest model
│   └── nutrition_model.pkl    # Nutrition regression model
│
└── data/
    ├── FOOD-DATA-GROUP1.csv   # Nutrition dataset (cheeses, baked)
    ├── FOOD-DATA-GROUP2.csv   # Nutrition dataset (meats, fish)
    ├── FOOD-DATA-GROUP3.csv   # Nutrition dataset (vegetables, fruits)
    ├── FOOD-DATA-GROUP4.csv   # Nutrition dataset (beverages)
    └── FOOD-DATA-GROUP5.csv   # Nutrition dataset (snacks, fast food)
```

> **Note:** `food_subset/` (training images), `.venv/`, and `.env` are excluded from GitHub. See `.gitignore`.

---

## 📦 Datasets Used

### 1. Food Image Dataset (Training)
- **Source:** Food-101 (subset)
- **Classes:** 9 food categories
  - `cake` · `donuts` · `french_fries` · `fried_rice` · `hot_dog` · `ice_cream` · `momos` · `pizza` · `waffles`
- **Total Images:** ~2,943
- **Location:** `food_subset/` *(not uploaded to GitHub — download separately)*

### 2. Nutrition Dataset
- **Source:** FOOD-DATA-GROUP CSV files
- **Total Records:** 2,395 food items across 5 CSV files
- **Columns:** 37 nutritional attributes including:
  - Macronutrients: Calories, Protein, Carbohydrates, Fat, Fiber, Sugars
  - Vitamins: A, B1, B2, B3, B5, B6, B11, B12, C, D, E, K
  - Minerals: Calcium, Iron, Magnesium, Potassium, Zinc, Phosphorus, Copper
  - Nutrition Density score

---

## 🛠️ Technologies Used

| Category | Technology |
|---|---|
| **Language** | Python 3.10+ |
| **Web Framework** | Streamlit 1.45 |
| **ML / CV** | scikit-learn, scikit-image, OpenCV |
| **Feature Extraction** | HOG (Histogram of Oriented Gradients), HSV Color Histograms |
| **Classifier** | Random Forest (300 estimators) |
| **LLM / Chatbot** | Groq API + Llama 4 Scout 17B |
| **Data** | Pandas, NumPy |
| **Model Persistence** | joblib |
| **UI Styling** | Custom CSS, Google Fonts (Inter) |

---

## ⚙️ Installation

### Prerequisites
- Python 3.10 or higher
- A [Groq API key](https://console.groq.com) (free)

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/NutriVisionAI.git
cd NutriVisionAI

# 2. Create virtual environment
python -m venv .venv

# 3. Activate virtual environment
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

# 4. Install dependencies
pip install -r requirements.txt

# 5. Set your Groq API key
# Create a .env file in the project root:
echo GROQ_API_KEY=your_key_here > .env

# 6. (Optional) Retrain the model
python train_food_classifier.py
```

> **Important:** You must have the `food_subset/` directory with training images to retrain the model. The pre-trained `models/food_classifier.pkl` is included in the repository.

---

## 🚀 How to Run

```bash
streamlit run app.py
```

Open your browser at: **http://localhost:8501**

### Usage
1. Upload a food image (JPG, JPEG, or PNG)
2. The model automatically classifies the food and shows confidence %
3. If the prediction is wrong, use the **correction dropdown**
4. View real nutritional values from the CSV dataset
5. Read health outcomes and ratings
6. Ask questions in the **Nutrition Chatbot**

---

## 📸 Screenshots

> *Add screenshots here after deployment*

| Food Recognition | Nutritional Breakdown |
|---|---|
| *(screenshot)* | *(screenshot)* |

| Health Analysis | Nutrition Chatbot |
|---|---|
| *(screenshot)* | *(screenshot)* |

---

## 🔑 Environment Variables

| Variable | Description | Required |
|---|---|---|
| `GROQ_API_KEY` | Your Groq API key from [console.groq.com](https://console.groq.com) | ✅ Yes |

**Never commit your `.env` file to GitHub.** It is already excluded in `.gitignore`.

For Streamlit Cloud deployment, add the key under **App Settings → Secrets**.

---

## ☁️ Deployment — Streamlit Cloud

1. Push this repository to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Click **New App** → select your repo → set `app.py` as main file
4. Go to **Advanced Settings → Secrets** and add:
   ```toml
   GROQ_API_KEY = "your_groq_api_key_here"
   ```
5. Click **Deploy**

---

## 🔮 Future Improvements

- [ ] Upgrade classifier to CNN / EfficientNet for higher accuracy
- [ ] Add barcode scanning for packaged food
- [ ] Support meal planning and daily calorie tracking
- [ ] Add user authentication and history persistence
- [ ] Mobile-responsive layout
- [ ] Support more food classes (expand to Food-101 full dataset)
- [ ] Integration with wearables for personalized recommendations

---

## 👩‍💻 Author

**Ekshitha**
- 📧 *[Your Email]*
- 🔗 *[Your LinkedIn]*
- 🐙 *[Your GitHub Profile]*

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">
Made with ❤️ using Streamlit + Groq + scikit-learn
</div>
