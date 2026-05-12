# CareCompanion - Agentic Healthcare AI Assitant

CareCompanion AI is an autonomous agentic system that acts as a personal health companion for patients managing Type 2 Diabetes. Unlike a regular chatbot, it remembers patients across sessions, calls real healthcare APIs to fetch live data, and proactively surfaces FDA drug alerts without being asked.
The agent reasons across three domains simultaneously - medications, nutrition, and cost — and generates personalized responses grounded in the patient's actual medical profile.

## Key Features

- Persistent memory — remembers patient history across sessions using ChromaDB vector store + SQLite
- 5 real healthcare APIs — OpenFDA, RxNorm, USDA FoodData, PubMed, GoodRx pricing
- Autonomous tool selection — agent decides which APIs to call based on patient query
- Proactive FDA monitoring — surfaces drug safety alerts without being asked
- Cost optimization — identifies generic medication alternatives and annual savings
- Nutrition assessment — flags foods by glycemic impact for diabetes management
- Multi-patient support — 70 simulated patient profiles with realistic clinical data


## Architecture

```
User Message
     │
     ▼
┌─────────────────┐
│  Node 1         │  Load patient profile (SQLite)
│  load_context   │  Retrieve relevant past conversations (ChromaDB)
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Node 2         │  LLM decides which tools are needed
│  decide_tools   │  based on patient profile + message
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌──────────────────────────────────────┐
│  Node 3         │────▶│  OpenFDA  · RxNorm  · USDA           │
│  run_tools      │     │  PubMed   · GoodRx pricing           │
└────────┬────────┘     └──────────────────────────────────────┘
         │
         ▼
┌─────────────────┐
│  Node 4         │  Generates personalized response
│  generate       │  Saves to memory · Writes agent note
└─────────────────┘
```

## Memory Architecture:

SQLite — structured patient facts (medications, glucose, BMI, budget, insurance)
ChromaDB — conversation history as vectors for semantic search across sessions

## Project Structure

```
CareCompanion/
│
├── codes/
│   ├── data.ipynb       
│   ├── memory_arch.ipynb  
│   ├── api.ipynb       
│   ├── agent.ipynb             
│   ├── ui.ipynb            
│   └── evaluation.ipynb           
│
├── data/
│   ├── raw/                          # Pima Diabetes dataset (768 patients)
│   └── processed/
│       ├── diabetes_clean.csv        
│       └── distributions.png         
│
├── memory/
│   ├── sqlite/carecompanion.db       # Patient profiles + structured facts
│   └── chroma_db/                    # Conversation vector store
│
├── evaluation/
│   ├── test_patients.json            
│   ├── test_patients_expanded.json   
│   ├── metrics_report.json           
│   └── metrics_dashboard.png        
│
└── requirements.txt
```

## Dataset

- Pima Indians Diabetes Database — 768 real patient records (Kaggle/UCI)
- 70 simulated test patients — derived from Pima dataset with realistic medication regimens, insurance types, budgets, and comorbidities
- USDA FoodData Central — 300k+ foods with full nutritional profiles


## Quickstart

1. Clone the repo
bashgit clone https://github.com/maithili74/CareCompanion
```
cd carecompanion-ai
```

3. Install dependencies
```
pip install -r requirements.txt
```

5. Get free API keys

Groq: https://console.groq.com (instant, no card)
USDA: https://fdc.nal.usda.gov/api-guide.html (instant)
OpenFDA: no key needed
PubMed: no key needed

4. Set your keys
```
os.environ["GROQ_API_KEY"] = "your_key_here"
os.environ["USDA_API_KEY"] = "your_key_here"
```
5. Run notebooks in order
```
data -> memory_arch -> api -> agent -> ui
```

6. Launch the UI
Run all cells in ui.ipynb, a local + public URL will appear automatically.
