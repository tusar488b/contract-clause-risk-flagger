# Contract Clause Risk Flagger

An AI-powered contract analysis workflow built with **n8n**, **Google Gemini**, and **Google Sheets**.

## 📌 Project Overview

Contract Clause Risk Flagger automatically analyzes a contract and identifies potentially risky clauses.

The workflow uses Google Gemini to examine contract terms and classify risks based on areas such as termination, payment terms, liability, confidentiality, intellectual property, and dispute resolution.

The identified risks are structured and stored in Google Sheets for easy review.

## 🚀 Features

* AI-powered contract clause analysis
* Automatic risk identification
* Risk classification: LOW, MEDIUM, HIGH
* Clause-by-clause risk analysis
* Risk type identification
* Risk reason and explanation
* AI-generated recommendations
* Automatic Google Sheets reporting
* Structured JSON processing
* n8n workflow automation

## 🔄 Workflow

```text
Contract Input
      ↓
Google Gemini
      ↓
Parse Contract Risk JSON
      ↓
Format Risk Items
      ↓
Risk Level Check
      ↓
Google Sheets
      ↓
Final Risk Report
```

## 🧠 Risk Categories

The workflow checks for potential risks involving:

* Termination rights
* Payment terms
* Liability
* Indemnification
* Intellectual property
* Confidentiality
* Non-compete clauses
* Automatic renewal
* Dispute resolution
* Governing law
* Data privacy
* One-sided obligations
* Unlimited liability
* Deadlines and penalties

## 🛠️ Technologies

* **n8n** — Workflow automation
* **Google Gemini** — AI contract analysis
* **Google Sheets** — Risk reporting
* **JavaScript** — JSON parsing and data transformation

## 📊 Example Output

| Clause | Risk Level | Risk Type             |
| ------ | ---------- | --------------------- |
| 1      | HIGH       | Termination           |
| 3      | HIGH       | Liability             |
| 6      | HIGH       | Dispute Resolution    |
| 2      | MEDIUM     | Payment Terms         |
| 4      | MEDIUM     | Confidentiality       |
| 5      | MEDIUM     | Intellectual Property |

Each identified risk includes:

* Clause number
* Original clause text
* Risk level
* Risk type
* Risk reason
* Recommended improvement

## 🎯 Use Case

## 🎯 Use Case

This automation can help freelancers, businesses, and contract-review teams quickly identify potentially problematic contract language before detailed human or legal review.

> **Note:** This project is an AI-assisted risk-flagging tool and does not provide legal advice. Contract decisions should be reviewed by a qualified legal professional.

## 👨‍💻 Author

**Tusar Biswas**

AI Automation Specialist

**Skills:** n8n · AI Agents · API Integration · Workflow Automation · RAG · Google Gemini
