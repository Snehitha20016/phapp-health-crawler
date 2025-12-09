# PHapp Health Resource Crawler

**Student Project for Wehealth Public Benefit Corporation**

## Project Overview

This project implements a reproducible data pipeline to discover, collect, and standardize location-based public health resources from California county health department websites. The crawler extracts contact information, facility details, and health service data to power PHapp's community resource features.

### Key Features
- Ethical web scraping with rate limiting and robots.txt compliance
- Automatic data categorization (CONTACT_INFO, LOCATION, FACILITY, SERVICE)
- Health topic tagging (vaccination, COVID, mental health, etc.)
- Data quality validation and deduplication
- Multiple output formats (CSV, JSON)
- Comprehensive logging and error handling

---

### Prerequisites
- Python 3.8 or higher
- pip package manager
- Internet connection for web scraping

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/phapp-health-crawler.git
cd phapp-health-crawler
```

2. **Create virtual environment:**
```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On Mac/Linux:
source venv/bin/activate
```

3. **Install dependencies:**
```bash
pip install -r requirements.txt
```

---

## Usage

### Option 1: Run in Jupyter Notebook (Recommended)

1. Start Jupyter:
```bash
jupyter notebook
```

2. Open `Project-Deliverables_SGorantla.ipynb`

3. Run all cells sequentially:
   - Cell 1: Import libraries
   - Cells 2-10: Web Scraping Fundamentals (demonstration)
   - Cells 11-12: Building Python Crawler (demonstration)
   - Cells 13-14: Data Processing & Categorization (demonstration)
   - Cells 15-16: Complete Pipeline Execution

### Option 2: Run as Python Script
```python
from phapp_pipeline import PHappPipeline

# Initialize pipeline
pipeline = PHappPipeline(verbose=True)

# Load URLs from CSV
urls = pipeline.load_urls_from_csv('data/us-ca.csv')

# Run complete pipeline
pipeline.run_pipeline(urls, output_dir='output')
```

---

## Project Components

### 1. HealthCrawler Class
**Purpose:** Fetch and extract data from health department websites

**Key Methods:**
- `fetch_page(url)` - HTTP request with error handling
- `parse_html(html)` - BeautifulSoup parsing
- `extract_phones(soup)` - Regex-based phone extraction
- `extract_emails(soup)` - Email extraction
- `extract_addresses(soup)` - Address extraction
- `crawl(url)` - Complete single-site crawl
- `batch_crawl(urls)` - Process multiple sites

**Features:**
- 2-second delay between requests (rate limiting)
- Comprehensive error handling (timeout, connection, HTTP errors)
- Session management for efficient requests
- Logging for debugging

### 2. DataProcessor Class
**Purpose:** Clean, standardize, and categorize scraped data

**Key Methods:**
- `clean_phone(phone)` - Standardize to (XXX) XXX-XXXX
- `clean_email(email)` - Lowercase and trim
- `clean_address(address)` - Normalize abbreviations
- `categorize_resource(resource)` - Auto-categorization
- `tag_health_topics(text)` - Keyword-based tagging
- `process_scraped_data(data)` - Complete processing pipeline
- `remove_duplicates(df)` - Deduplication
- `generate_quality_report(df)` - Metrics generation

**Categories:**
- CONTACT_INFO: phones, emails
- LOCATION: addresses, geographic data
- FACILITY: clinics, hospitals, centers
- SERVICE: vaccinations, testing, screening

**Health Topics Detected:**
- vaccination, covid, flu, mental_health, dental
- pediatric, maternal, emergency, chronic_disease, substance_abuse

### 3. PHappPipeline Class
**Purpose:** Orchestrate complete workflow from CSV to outputs

**Workflow:**
1. Load URLs from CSV (auto-detect column names)
2. Crawl all websites with progress tracking
3. Process and clean data with DataProcessor
4. Remove duplicates
5. Generate outputs (CSV, JSON, reports, catalog)

---

## Output Files

### Generated Automatically:

1. **raw_crawled_data.json**
   - Complete scraped data with timestamps
   - Includes success/failure status per URL
   - Contains all extracted phones, emails, addresses

2. **health_resources_clean.csv** 
   - Cleaned, deduplicated data in tabular format
   - Ready for analysis in Excel, R, or other tools

3. **health_resources_clean.json** 
   - Same data in JSON format
   - Suitable for API integration

4. **data_dictionary.md** 
   - Complete field descriptions
   - Data types and formats
   - Example values

5. **quality_report.txt**
   - Success rate metrics
   - Category distribution
   - Health topic counts
   - Data completeness percentages

6. **source_catalog.csv** 
   - Source URL tracking
   - Last scraped timestamps
   - Refresh strategies
   - Access notes

---

## Data Dictionary

See `output/data_dictionary.md` for complete field descriptions.

**Quick Reference:**
- `source_url`: Origin website (string)
- `facility_name`: Health department name (string)
- `phone_clean`: Standardized phone (XXX) XXX-XXXX (string)
- `email_clean`: Lowercase email (string)
- `address_clean`: Normalized address (string)
- `category`: Resource type (enum: CONTACT_INFO/LOCATION/FACILITY/SERVICE)
- `health_topics`: Identified topics (list of strings)
- `scraped_at`: Collection timestamp (ISO 8601)
- `processed_at`: Processing timestamp (ISO 8601)

---

