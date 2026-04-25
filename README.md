Mantra Identification Tool
Overview
This tool extracts and compares mantras from two documents using a dual-layer similarity engine:

Semantic similarity — sentence-transformer embeddings (all-MiniLM-L6-v2) + cosine distance
Lexical similarity — TF-IDF (unigram + bigram) + cosine distance

Mantras are short, self-contained textual units (5–200 characters) detected automatically from your documents. The tool ranks every pairing and lets you filter results by a configurable similarity threshold.

Setup Instructions
1. Clone Repository
bashgit clone <repo_url>
cd mantra_app
2. Create Virtual Environment
bashpython3 -m venv venv
source venv/bin/activate        # macOS / Linux
# venv\Scripts\activate         # Windows
3. Install Dependencies
bashpip install -r requirements.txt

Note: The first run will download the all-MiniLM-L6-v2 model (~90 MB) from Hugging Face. An internet connection is required for this step; subsequent runs use the cached model.


Run Application
bashstreamlit run app.py
The app opens automatically in your browser at http://localhost:8501.

Usage

Upload Document A — click the left uploader and select a file
Upload Document B — click the right uploader and select a file
Adjust the Minimum Semantic Score slider if desired (default 0.30)
Click ⚡ Run Analysis
Review the ranked match table; export results with ⬇ Export results as CSV


Supported Formats
FormatExtensionPlain text.txtPDF.pdfWord document.docx

Project Structure
mantra_app/
│
├── app.py                  # Streamlit GUI + controller
├── requirements.txt        # Python dependencies
├── README.md
│
├── core/
│   ├── extractor.py        # File parsing (TXT / PDF / DOCX)
│   ├── preprocess.py       # Text normalisation
│   ├── mantra_detector.py  # Heuristic mantra extraction
│   ├── similarity.py       # Semantic + lexical similarity matrices
│   └── matcher.py          # Top-K matching engine
│
└── utils/
    └── helpers.py          # Shared utility functions

Performance Notes

The sentence-transformer model is loaded once globally — repeated analyses in the same session do not reload it.
Documents up to ~1 000 lines process in well under 10 seconds after the initial model load.
Results are filterable by semantic score to surface only the most meaningful matches.
