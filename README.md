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
