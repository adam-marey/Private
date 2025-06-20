# AI Agent for Medical Device JSON Conversion

This project is designed to convert raw JSON data from FDA-approved RPM devices (like blood pressure cuffs) into a standardized format using an AI-powered multi-agent system.

The agent identifies device types, fetches documentation if needed, and applies smart field mapping — with fallback refinement using LLMs.

---

## Features

- Auto-detect device model from raw JSON
- Scrape online documentation for unknown formats
- Retry failed conversions using GPT/Claude
- Validate and standardize output for storage
- Supports real-time webhook + queued processing

---

## 🔄 Workflow Overview

1. Raw JSON submitted (via webhook or API)
2. Agent checks if device format is already known
3. If found → convert using cached mapping
4. If unknown → search web + scrape device docs
5. Convert data to standard JSON format
6. Validate + refine (via LLM if needed)
7. Store converted data in MongoDB or ... ?
8. Return success/failure

---

## AI Agents

### 1. Device Context Agent
- Checks cache/vector DB for known format
- Returns mapping if available

### 2. Documentation Fetcher
- Uses Brave API or Playwright to search/scrape docs
- Parses HTML or PDFs to extract field info

### 3. JSON Conversion Agent
- Maps raw keys to a clean, standardized JSON output
- Based on field mapping from cache or docs

### 4. Refinement Agent
- Validates schema
- Uses LLM to resolve unclear fields or fix errors
- Retries conversion with improved mappings

---

## Tech Stack

| Purpose        | Tech                         |
|----------------|------------------------------|
| Language       | Python                       |
| API Framework  | FastAPI                      |
| Queue System   | Redis or Celery              |
| LLMs           | GPT-4o / Claude 3            |
| Search         | Brave Search API             |
| Scraping       | Playwright / BeautifulSoup   |
| Database       | MongoDB                      |
| Vector DB (optional) | Pinecone / FAISS       |

---

## Sample Output Format

```json
{
  "DeviceId": "OM123456",
  "MeasurementType": "Blood Pressure",
  "Values": [
    {"Type": "systolic", "Value": 130},
    {"Type": "diastolic", "Value": 80},
    {"Type": "pulse", "Value": 72}
  ],
  "MeasuredAt": "2025-06-17T12:30:00Z",
  "PatientId": "patient-001"
}
```

## Development Checklist

- [ ] `/convert` API endpoint for JSON ingestion  
- [ ] Device context lookup module  
- [ ] Web search & scraping logic  
- [ ] JSON converter with field mapping  
- [ ] LLM-powered refinement layer  
- [ ] MongoDB integration  ... 
- [ ] Logging + retry logic for failed conversions  

