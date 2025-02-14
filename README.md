# 💸 Smart Expense Tracker with NLP

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg)
![HuggingFace](https://img.shields.io/badge/Hugging%20Face-Transformers-yellow)
![SQLite](https://img.shields.io/badge/SQLite-07405E.svg)

A GPU-accelerated expense tracker that understands natural language! Automatically categorizes expenses and answers financial queries using **Microsoft's Phi-2 model** and SQLite.

---

## ✨ Core Features
- **Auto-Categorization**: Detects `coffee`, `food`, `travel`, `groceries`, and `shopping` using keywords.
- **NLP Classification**: Uses Phi-2 to distinguish between expense entries ("₹200 for pizza") and queries ("What did I spend on coffee?").
- **Dynamic Queries**:  
  - Total expenses (by category/day/week)
  - Biggest expenses
  - Yesterday's spending
  - Online food orders (Swiggy/Blinkit)
- **Date Intelligence**: Handles relative dates like "yesterday" and ISO date formatting.

---

## 🛠️ Technical Breakdown
### Pipeline Setup
1. **GPU Optimization**: Uses `torch.cuda` and `GPUtil` for device management.
2. **Model Loading**: Initializes `microsoft/phi-2` via Hugging Face Transformers.
3. **SQLite Database**: Stores data with schema:
   ```sql
   (id, description, amount, date, categories)
   ```

### Key Functions
- **`extract_expense_details`**: Regex-based amount detection (`₹`, `Rs`, `INR`) + keyword categorization.
- **`classify_message`**: Hybrid rule-based + LLM classification (Phi-2) for expense/query detection.
- **`process_query`**: SQL operations for filtering by date, category, or amount.

---

## 📊 Example Workflow
**Expense Entry**  
`"Bought Levi's jeans for ₹1500"`  
→  
```
Expense added: ₹1500 on 2023-12-01.  
Details: Bought Levi's jeans for ₹1500 | Categories: ['shopping']
```

**Natural Language Query**  
`"Show my biggest expenses this week"`  
→  
```
Your biggest expenses this week are:  
  • Dinner at fine dining restaurant | ₹1800 | 2023-12-03 | Categories: food  
  • Uber to airport | ₹600 | 2023-12-02 | Categories: travel  
```

---

## ⚡ GPU/CPU Handling
- Prioritizes CUDA acceleration if available (via `torch.cuda.is_available()`).
- Falls back seamlessly to CPU with warning logs.
- Uses `pipeline("text-generation", device=0)` for Phi-2 inference.

---

###### *Note: Requires `transformers`, `sqlite3`, and `torch` (with CUDA toolkit for GPU support).*
```
