# 💸 Smart Expense Tracker with NLP

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg)
![HuggingFace](https://img.shields.io/badge/Hugging%20Face-Transformers-yellow)
![SQLite](https://img.shields.io/badge/SQLite-07405E.svg)

A **GPU-accelerated expense tracker** that understands natural language! This project leverages **Microsoft's Phi-2 model** and **SQLite** to automatically categorize expenses and answer financial queries in a conversational manner. Whether you're adding expenses or asking about your spending habits, this system makes it seamless and intuitive.

---

## ✨ Core Features

### 1. **Automatic Expense Categorization**
- Detects categories like `coffee`, `food`, `travel`, `groceries`, and `shopping` using **keyword matching**.
- Example:  
  - Input: `"Had a filter coffee at a local café, cost ₹50."`  
  - Output: `Categories: ['coffee', 'food']`

### 2. **Natural Language Processing (NLP)**
- Uses **Microsoft's Phi-2 model** to classify messages as either **expense entries** or **queries**.
  - Expense Entry: `"₹200 for pizza"` → Classified as `expense`.
  - Query: `"What did I spend on coffee?"` → Classified as `query`.

### 3. **Dynamic Query Handling**
- Supports a variety of natural language queries:
  - **Total Expenses**: `"What is my total expense so far?"`
  - **Category-wise Spending**: `"How much have I spent on coffee?"`
  - **Date-based Queries**: `"What are my expenses from yesterday?"`
  - **Biggest Expenses**: `"Show my biggest expenses this week."`
  - **Online Food Orders**: `"How much did I spend on Swiggy or Blinkit?"`

### 4. **Date Intelligence**
- Automatically handles relative dates like **"yesterday"** and converts them into ISO format (`YYYY-MM-DD`).
- Example:  
  - Input: `"Bought snacks from Blinkit for ₹300 yesterday."`  
  - Output: `Date: 2023-12-01` (if today is 2023-12-02).

---

## 🛠️ Technical Breakdown

### 1. **Pipeline Setup**
- **GPU Optimization**: Uses `torch.cuda` and `GPUtil` to check for GPU availability and optimize performance.
- **Model Loading**: Initializes the `microsoft/phi-2` model via Hugging Face's `transformers` library.
- **SQLite Database**: Stores all expense data in a structured format with the following schema:
  ```sql
  (id, description, amount, date, categories)
  ```

### 2. **Key Functions**
- **`extract_expense_details`**:
  - Extracts the **amount** using regex (supports `₹`, `Rs`, `INR`).
  - Automatically categorizes expenses based on keywords (e.g., "Starbucks" → `coffee`).
  - Example:  
    - Input: `"Dinner at a fine dining restaurant, cost ₹1800."`  
    - Output: `{'amount': 1800, 'description': 'Dinner at a fine dining restaurant', 'date': '2023-12-01', 'categories': '["food"]'}`

- **`classify_message`**:
  - Uses a **hybrid approach** (rule-based + Phi-2 model) to classify messages as `expense` or `query`.
  - Example:  
    - Input: `"How much did I spend on groceries?"`  
    - Output: `query`

- **`process_query`**:
  - Executes SQL queries to filter and retrieve data based on user requests.
  - Example:  
    - Query: `"What is my total expense on travel?"`  
    - SQL: `SELECT SUM(amount) FROM expenses WHERE categories LIKE '%travel%'`

---

## 📊 Example Workflow

### 1. **Adding Expenses**
- Input: `"Bought Levi's jeans for ₹1500"`  
- Output:  
  ```
  Expense added: ₹1500 on 2023-12-01.  
  Details: Bought Levi's jeans for ₹1500 | Categories: ['shopping']
  ```

### 2. **Querying Expenses**
- Query: `"Show my biggest expenses this week"`  
- Output:  
  ```
  Your biggest expenses this week are:  
    • Dinner at fine dining restaurant | ₹1800 | 2023-12-03 | Categories: food  
    • Uber to airport | ₹600 | 2023-12-02 | Categories: travel  
  ```

- Query: `"How much have I spent on coffee?"`  
- Output:  
  ```
  Your total coffee expenditure is: ₹630  
  Detailed Coffee Expenses:  
    • Had a filter coffee... | ₹50 | 2023-12-01 | Categories: coffee, food  
    • Spent ₹400 on a cappuccino... | ₹400 | 2023-12-01 | Categories: coffee  
  ```

---

## ⚡ GPU/CPU Handling
- **GPU Acceleration**: Prioritizes CUDA if available (via `torch.cuda.is_available()`).
- **Fallback to CPU**: If GPU is unavailable, the system seamlessly switches to CPU with a warning log.
- **Model Inference**: Uses Hugging Face's `pipeline("text-generation", device=0)` for Phi-2 inference.

---

## 🚀 Getting Started
1. **Install Dependencies**:
   ```bash
   pip install transformers sqlite3 torch GPUtil
   ```
2. **Run the Notebook**:
   - Execute the notebook to initialize the database, load the model, and start tracking expenses.
3. **Interact with the Tracker**:
   - Add expenses or ask queries in natural language.

---

###### *Note: Requires `transformers`, `sqlite3`, and `torch` (with CUDA toolkit for GPU support).*
