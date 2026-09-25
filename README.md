Readify - On-Device OCR & Document AI

100% Private, Zero-API, Client-Side Document Intelligence Engine

Extract structured financial data, invoices, retail receipts, line items, and tax identifiers directly in your browser using WebAssembly and local regex heuristics.


📌 Table of Contents

Overview

Key Features

Architecture & How It Works

Extracted Data Schema

Getting Started

User Workflow

Privacy & Security

Tech Stack

Roadmap & Future Enhancements

License


🪟 Overview

Readify is a single-page WebAssembly-powered application designed to automate document parsing and optical character recognition (OCR) without sending sensitive document images or personal financial data to third-party cloud APIs.

Whether processing tax invoices, retail store receipts, purchase orders, or bills, Readify runs Tesseract V5 (compiled to WebAssembly) locally in the user's browser context. It extracts text, parses key metadata, structures line items, calculates financial subtotals, and exposes the data via an interactive interface and export utilities.


✨ Key Features

🔒 100% On-Device & Private

Zero API Dependencies: No Google Gemini, OpenAI, or AWS Textract API keys required.

Client-Side WASM: Tesseract.js V5 runs inside a web worker entirely in browser memory.

Air-Gapped Ready: Works offline once assets are cached.

🖼️ Document Input & Pre-Processing

File Support: Supports Drag & Drop and file picker for PNG, JPG, JPEG, and WEBP.

Pre-Processing Pipeline: Built-in canvas engine supporting:

90° Canvas Rotation for misaligned documents.

Grayscale conversion with dynamic contrast boosting to maximize OCR accuracy.

Fullscreen Zoom & Inspection modal.

Instant Sample Generators: Generate synthetic high-resolution GST Tax Invoices or Retail Store Bills on canvas with one click for rapid testing.


🧠 Deterministic Heuristic Extraction

Entity Identification: Extracts Supplier/Merchant and Buyer/Customer names, GSTINs (Indian Goods & Services Tax Identification Numbers), and PAN identifiers using optimized regex patterns.

Metadata Capture: Parses Document Types, Invoice/Receipt Numbers, Dates, Times, and Payment Modes (Net Banking, UPI, Cash, Credit Card).

Line Item Extraction: Automatically detects tabular data to create itemized breakdowns complete with Description, HSN/SAC codes, Quantities, Unit Rates, Tax %, and Line Totals.

Financial Calculation: Summarizes Subtotals, CGST, SGST, IGST, Discounts, and Grand Totals.



🛠️ Interactive UI Workspace

Structured Overview Panel: Visual layout with metadata cards, entity blocks, interactive line item tables, and financial totals.

Editable Form View: Syncs extracted data with form inputs so users can refine or correct field values.

Raw OCR Text & Live Search: View raw extracted OCR text with confidence metrics and real-time query highlighting.

Raw JSON Viewer: View and copy the structured JSON representation.

Inline Table Editing: Add, delete, or modify line items on the fly with live total auto-recalculation.

📤 Multi-Format Export Capabilities

CSV Export: Download itemized table rows and financial summaries in .csv format.

JSON Export: Download structured document object in .json format.

Print / PDF Export: Styled print view optimized for generating clean physical or PDF document reports.


🏗 Architecture & How It Works

┌─────────────────────────────────────────────────────────────────┐
│                      User Browser Window                        │
├─────────────────┬──────────────────────────────┬────────────────┤
│ 1. Document     │ 2. Pre-processing            │ 3. OCR Engine  │
│    Input        │    Canvas                    │    (WASM Worker)│
│                 │                              │                │
│  [Upload / Drop]│ ──> [Rotation / Grayscale] ──>│ [Tesseract V5] │
│  [Sample Gen]   │                              │  Extracts Text │
└─────────────────┴──────────────────────────────┴───────┬────────┘
                                                         │
                                                         ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. Deterministic Local Parsing Engine (Regex / Heuristics)       │
│                                                                 │
│  • GSTIN: /\b[0-9]{2}[A-Z]{5}[0-9]{4}[A-Z]{1}[1-9A-Z]{1}Z...\b/ │
│  • Dates: /\b\d{1,2}[\/\.-]\d{1,2}[\/\.-]\d{2,4}\b/             │
│  • Line Items: Item name, Qty, Rate, Total                      │
└────────────────────────────────┬────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. Workspace UI & Exporters                                     │
│                                                                 │
│  [ Structured View ]  [ Edit Form ]  [ Raw OCR ]  [ JSON View ] │
│                                                                 │
│                 --->  [ CSV | JSON | PDF Export ]               │
└─────────────────────────────────────────────────────────────────┘



Image Ingestion: User drops an image or selects a pre-built canvas sample.

Pre-Processing Canvas: The image is drawn onto an HTML5 canvas where contrast and rotation transformations are applied.

WASM Execution: Tesseract.createWorker() processes the canvas buffer inside a background thread, yielding text tokens and confidence scores.

Pattern Analysis: A set of JavaScript regular expressions extracts structured values from raw text lines.

State Rendering: Extracted data feeds into an interactive UI state manager, rendering data grids, tables, forms, and exports.


📊 Extracted Data Schema

Readify parses document image data into the following standard JSON format:

{
  "documentInfo": {
    "documentType": "Tax Invoice",
    "documentNumber": "INV-2026-904",
    "date": "25/09/2026",
    "time": "18:42",
    "paymentMethod": "Net Banking / UPI"
  },
  "supplier": {
    "companyName": "NEXUS DIGITAL LABS PVT LTD",
    "gstinTaxId": "29AABCN8891M1Z5",
    "panRegistration": "AAAAA0000A",
    "phone": "+91 98765 43210"
  },
  "buyer": {
    "customerName": "Acme Enterprises India Ltd",
    "gstinTaxId": "27AAACA9921B1Z2",
    "phone": "+91 91234 56789"
  },
  "lineItems": [
    {
      "slNo": 1,
      "description": "Cloud Server Hosting (Monthly)",
      "hsnSacCode": "998313",
      "quantity": "1",
      "unitPrice": "12000.00",
      "taxRatePercent": "18",
      "totalAmount": "12000.00"
    }
  ],
  "financials": {
    "subtotal": "23500.00",
    "cgst": "2115.00",
    "sgst": "2115.00",
    "igst": "0.00",
    "discount": "0.00",
    "grandTotal": "27730.00"
  }
}



🚀 Getting Started

Readify is built as a single, self-contained HTML web application. No complex server environments, node dependencies, or build tools are required to run it locally.

Prerequisites

Any modern Web browser (Chrome, Firefox, Edge, Safari, Brave) with WebAssembly and Web Workers enabled.

Running Locally

Clone or Download the Repository:

git clone https://github.com/your-username/readify-doc-ai.git
cd readify-doc-ai


Serve the Application:
Because Tesseract.js loads WebAssembly worker files via CDN, serve the index.html file through a local web server (such as Python's HTTP server or VS Code Live Server):

Using Python 3:

python -m http.server 8000


Using Node.js npx serve:

npx serve .

Open in Browser:
Navigate to http://localhost:8000 or http://localhost:3000.



🔄 User Workflow

Load Document:

Drag and drop a receipt or invoice image into the drop area, or click GST Tax Invoice / Retail Store Bill to test with generated samples.

Optimize Image (Optional):

Use the pre-processing toolbar to rotate the image or enable Grayscale / Contrast enhancement if the image is poorly lit or angled.

Run Extraction:

Click Run On-Device AI Extraction. Watch the real-time HUD progress bar as Tesseract initializes and scans the file.


Review & Edit:

Structured Data: Inspect metadata cards, entity details, and line item tables.

Edit Form: Modify document numbers, entity details, or add/delete line items manually.

Raw OCR Text: Search specific terms or check OCR accuracy ratings.



Export:

Click CSV, JSON, or Print to export your structured data.

🔒 Privacy & Security

Readify is built with a privacy-first design:

No Remote Servers: All parsing, canvas rendering, and OCR execution occur locally in browser memory.

Data Compliance: Ideal for organizations dealing with confidential financial records, HIPAA, GDPR, or strict internal data sovereignty policies.

Zero Tracking: No analytics tracking scripts or remote payload telemetry.



🛠 Tech Stack

Frontend Framework: Vanilla HTML5, JavaScript (ES6+), CSS3

Styling & UI: Tailwind CSS (v3 via CDN)

Typography & Icons: FontAwesome 6, Google Fonts (Plus Jakarta Sans, JetBrains Mono)

OCR Engine: Tesseract.js V5 (WebAssembly worker engine)



🛣 Roadmap & Future Enhancements

[ ] Multi-Page PDF Processing: Support multi-page PDF documents via pdf.js.

[ ] Custom Regex Builder: Allow users to define custom regular expressions for specialized document types (medical bills, bills of lading, utility bills).

[ ] WebGPU Acceleration: Speed up OCR feature matching on modern GPUs.

[ ] Batch Processing: Support drag-and-drop for multiple document files with batch CSV export.


📄 License

Distributed under the MIT License. See LICENSE for more information.
