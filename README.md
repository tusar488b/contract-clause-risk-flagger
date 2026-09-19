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

This automation can help freelancers, businesses, and contract-review teams quickly identify potentially problematic contract language before detailed human or legal review.

> **Note:** This project is an AI-assisted risk-flagging tool and does not provide legal advice. Contract decisions should be reviewed by a qualified legal professional.

## 👨‍💻 Author

**Tusar Biswas**

AI Automation Specialist

**Skills:** n8n · AI Agents · API Integration · Workflow Automation · RAG · Google Gemini

{
  "nodes": [
    {
      "parameters": {},
      "type": "n8n-nodes-base.manualTrigger",
      "typeVersion": 1,
      "position": [
        0,
        0
      ],
      "id": "4c6c72ec-feb4-4331-8f51-cd2ad115c172",
      "name": "When clicking ‘Execute workflow’"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "d51d8fb1-9219-44f2-ac05-e754c5373028",
              "name": "contract_text",
              "value": "SERVICE AGREEMENT  1. Termination The Client may terminate this agreement at any time without notice and without providing any reason.  2. Payment The Client shall pay the Service Provider within 90 days after receiving an invoice.  3. Liability The Service Provider shall be fully responsible for any and all losses, damages, claims, costs, and expenses arising from the services, without any limitation.  4. Confidentiality The Service Provider must keep all confidential information strictly confidential during and after the agreement.  5. Intellectual Property All intellectual property created by the Service Provider during the project shall automatically belong exclusively to the Client.  6. Dispute Resolution Any dispute arising from this agreement shall be resolved through arbitration in a location selected solely by the Client.",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        224,
        0
      ],
      "id": "0cbfbbeb-6a5b-4b2a-8295-a8be5d7d148e",
      "name": "Contract Input"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "models/gemini-3.5-flash",
          "mode": "list",
          "cachedResultName": "models/gemini-3.5-flash"
        },
        "messages": {
          "values": [
            {
              "content": "=You are an expert contract risk analysis assistant.\n\nAnalyze the following contract and identify clauses that may create legal, financial, operational, or business risk.\n\nFor each risky clause, provide:\n\n1. clause_number\n2. clause_text\n3. risk_level: LOW, MEDIUM, or HIGH\n4. risk_type\n5. risk_reason\n6. recommendation\n\nFocus especially on:\n\n- Termination rights\n- Payment terms\n- Liability\n- Indemnification\n- Intellectual property\n- Confidentiality\n- Non-compete\n- Automatic renewal\n- Dispute resolution\n- Governing law\n- Data privacy\n- Unclear or one-sided obligations\n- Unlimited liability\n- Unreasonable deadlines or penalties\n\nDo not invent facts that are not present in the contract.\n\nReturn ONLY valid JSON.\n\nUse this exact structure:\n\n{\n  \"contract_summary\": \"short summary\",\n  \"overall_risk\": \"LOW | MEDIUM | HIGH\",\n  \"risk_count\": 0,\n  \"risks\": [\n    {\n      \"clause_number\": \"1\",\n      \"clause_text\": \"exact clause text\",\n      \"risk_level\": \"HIGH\",\n      \"risk_type\": \"Termination\",\n      \"risk_reason\": \"Why this clause creates risk\",\n      \"recommendation\": \"Suggested improvement\"\n    }\n  ]\n}\n\nContract:\n\n{{ $json.contract_text }}"
            }
          ]
        },
        "jsonOutput": "={{ false }}",
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.googleGemini",
      "typeVersion": 1.2,
      "position": [
        448,
        0
      ],
      "id": "3d69e7bb-8389-4fe0-a337-bdbdc8831662",
      "name": "Message a model",
      "credentials": {
        "googlePalmApi": {
          "id": "XGQxgmqat3DDmbMA",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const output = $input.first().json;\n\nlet text = output.content?.parts?.[0]?.text || \"\";\n\ntext = text\n  .replace(/^```json\\s*/i, \"\")\n  .replace(/^```\\s*/i, \"\")\n  .replace(/\\s*```$/i, \"\")\n  .trim();\n\nlet result;\n\ntry {\n  result = JSON.parse(text);\n} catch (error) {\n  throw new Error(\"AI returned invalid JSON: \" + error.message);\n}\n\nreturn [\n  {\n    json: result\n  }\n];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        800,
        0
      ],
      "id": "45ad4564-a027-426f-aa90-b04b6c8408bc",
      "name": "Parse Contract Risk JSON"
    },
    {
      "parameters": {
        "jsCode": "const data = $input.first().json;\n\nreturn data.risks.map(risk => ({\n  json: {\n    clause_number: risk.clause_number,\n    clause_text: risk.clause_text,\n    risk_level: risk.risk_level,\n    risk_type: risk.risk_type,\n    risk_reason: risk.risk_reason,\n    recommendation: risk.recommendation\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1024,
        0
      ],
      "id": "f7dd3c68-bfbf-4d33-a339-ee1ba62e8405",
      "name": "Format Risk Items"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "e9f686cc-add0-4414-b72e-dec0e860e746",
              "leftValue": "={{ $json.risk_level }}",
              "rightValue": "HIGH",
              "operator": {
                "type": "string",
                "operation": "equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        1248,
        0
      ],
      "id": "c5719a8a-a987-423f-aeb4-121ea81f4eb2",
      "name": "Risk Level Check"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44",
          "mode": "list",
          "cachedResultName": "Contract Risk Report",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "clause_number": "={{ $json.clause_number }}",
            "clause_text": "={{ $json.clause_text }}",
            "risk_level": "= {{ $json.risk_level }}",
            "risk_type": "= {{ $json.risk_type }}",
            "risk_reason": "= {{ $json.risk_reason }}",
            "recommendation": "={{ $json.recommendation }}"
          },
          "matchingColumns": [
            "clause_number"
          ],
          "schema": [
            {
              "id": "clause_number",
              "displayName": "clause_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "clause_text",
              "displayName": "clause_text",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "risk_level",
              "displayName": "risk_level",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "risk_type",
              "displayName": "risk_type",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "risk_reason",
              "displayName": "risk_reason",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "recommendation",
              "displayName": "recommendation",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1472,
        -96
      ],
      "id": "6f6a3d66-24b0-4135-a2b2-b8046592acbb",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "CCojuzk6BiJv6xTQ",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44",
          "mode": "list",
          "cachedResultName": "Contract Risk Report",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1UB-AhhhUq0HN9eMOt2EfJctALWeWuQ-VC4tIl9thI44/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "clause_number": "={{ $json.clause_number }}",
            "clause_text": "={{ $json.clause_text }}",
            "risk_level": "={{ $json.risk_level }}",
            "risk_type": "={{ $json.risk_type }}",
            "risk_reason": "={{ $json.risk_reason }}",
            "recommendation": "={{ $json.recommendation }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "clause_number",
              "displayName": "clause_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "clause_text",
              "displayName": "clause_text",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "risk_level",
              "displayName": "risk_level",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "risk_type",
              "displayName": "risk_type",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "risk_reason",
              "displayName": "risk_reason",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "recommendation",
              "displayName": "recommendation",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1472,
        96
      ],
      "id": "8b793e64-4912-4a0a-9189-238c66e51405",
      "name": "Append row in sheet1",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "CCojuzk6BiJv6xTQ",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        1696,
        0
      ],
      "id": "a358692b-b9f4-4580-993d-6ca13aedca58",
      "name": "Merge"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "a0b5c340-859f-427c-8462-e49cf2f251c4",
              "name": "report_status",
              "value": "Contract Risk Analysis Completed",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        1920,
        0
      ],
      "id": "a4b9c606-cfd4-4ba6-833b-1deb73ddabf6",
      "name": "Final Risk Report"
    }
  ],
  "connections": {
    "When clicking ‘Execute workflow’": {
      "main": [
        [
          {
            "node": "Contract Input",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Contract Input": {
      "main": [
        [
          {
            "node": "Message a model",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Message a model": {
      "main": [
        [
          {
            "node": "Parse Contract Risk JSON",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Parse Contract Risk JSON": {
      "main": [
        [
          {
            "node": "Format Risk Items",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Format Risk Items": {
      "main": [
        [
          {
            "node": "Risk Level Check",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Risk Level Check": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Append row in sheet1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet1": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "Final Risk Report",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Final Risk Report": {
      "main": [
        []
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "5817ea169fb15204ff3422eca7ca05108f45d4f182273cc3714afec5a6ff4c8b"
  }
}

