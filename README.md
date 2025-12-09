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



