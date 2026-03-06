🛡️ Agentic Financial Steward
Automated Invoice Audit & Reconciliation Terminal

Developed by Tianyi Li (UC Berkeley), this agentic system automates the high-integrity task of auditing corporate invoices against contracted rate cards. It leverages Small Language Models (SLMs) and Fuzzy String Matching to bridge the gap between messy, real-world OCR data and structured "Source of Truth" records.

🚀 Key Features
1. Intelligent OCR Extraction (Llama 3.2 + Instructor)

The system uses a local Llama 3.2 model via Ollama to extract structured JSON from unstructured PDF text.

Defensive Schema Parsing: Implemented a robust Pydantic Alias Layer to handle non-deterministic LLM outputs (e.g., mapping Total Price, total_price, and totalPrice to a single validated field).

Heuristic Data Cleaning: Custom validators automatically sanitize OCR noise, such as currency symbols ($), commas, and "Smart Quotes."

2. High-Precision Reconciliation (The "Decision Brain")

The reconciliation engine balances automation with risk management:

Two-Pass Fuzzy Matching: Utilizes token_set_ratio and partial_ratio to resolve complex entity mismatches (e.g., matching "Cable Mgmt Kit" to "Cable Management Kit").

5% Variance Threshold: Automatically flags any line item where the invoice price exceeds the contracted rate by more than 5%.

Vendor Verification: Strict character-ratio matching ensures only approved vendors are verified, preventing fraudulent or unauthorized billing.

3. Human-in-the-Loop "Discovery" Mode

For items not found in the Master Excel (Bundles) or new vendors, the system enters a "Discovery" state.

Non-Blocking Flags: Unseen items are logged as "Bundles" for manual review rather than failing the entire audit.

File Cabinet Visuals: A sidebar-integrated "File Cabinet" provides immediate color-coded status (Green/Red) for rapid audit oversight.

🛠️ Technical Stack
Language: Python 3.10+

LLM Framework: instructor + OpenAI (Local Ollama backend)

OCR/PDF: pdfplumber

Data Science: pandas, rapidfuzz (Levenshtein & Token Set algorithms)

Frontend: Streamlit

Reporting: fpdf2 (Binary PDF Generation)

📂 Project Structure
app.py: The Streamlit dashboard and UI logic.

main.py: The OCR Agent orchestration.

reconciler.py: The core reconciliation and fuzzy matching logic.

models.py: Pydantic data schemas and validation/alias layers.

📝 Performance Notes
Invoice 1 (Clean): Verified exact-match reconciliation and variance logic.

Invoice 2 (Messy OCR): Tested the resilience of the Alias Mapping and Data Cleaning layers.

Invoice 3 (New Vendor/Bundles): Demonstrated "Discovery" logic for registering unseen entities in the Master Excel.
