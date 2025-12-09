# PHapp Health Resource Data Dictionary
  
**Dataset:** California County Health Departments

---

## Overview

This data dictionary describes all fields in the cleaned health resource datasets (`health_resources_clean.csv` and `health_resources_clean.json`).

---

## Field Definitions

### 1. source_url
**Description:** The original URL where the data was collected  
**Data Type:** String (URL)  
**Format:** Full HTTP/HTTPS URL  
**Required:** Yes  
**Example:** `https://www.buttecounty.net/610/Public-Health`  
**Notes:** Used for attribution and data refresh tracking

---

### 2. facility_name
**Description:** Name of the health department or facility  
**Data Type:** String  
**Format:** Free text, Title Case  
**Required:** Yes  
**Example:** `Butte County Public Health Department`  
**Notes:** Extracted from page title or H1 heading

---

### 3. phone
**Description:** Raw phone number as found on website  
**Data Type:** String  
**Format:** Variable (as-scraped)  
**Required:** No (conditional)  
**Example:** `(530) 552-3800`, `530-552-3800`, `530.552.3800`  
**Notes:** Original format preserved for reference

---

### 4. phone_clean
**Description:** Standardized phone number  
**Data Type:** String  
**Format:** `(XXX) XXX-XXXX`  
**Required:** No (conditional)  
**Example:** `(530) 552-3800`  
**Transformation Rules:**
- Remove all non-digit characters
- Format 10-digit numbers as (XXX) XXX-XXXX
- Format 11-digit numbers starting with 1 by removing leading 1
- Invalid formats preserved as-is

---

### 5. email
**Description:** Raw email address as found on website  
**Data Type:** String  
**Format:** Standard email format  
**Required:** No (conditional)  
**Example:** `publichealth@buttecounty.net`  
**Notes:** Original format preserved

---

### 6. email_clean
**Description:** Standardized email address  
**Data Type:** String  
**Format:** Lowercase, trimmed  
**Required:** No (conditional)  
**Example:** `publichealth@buttecounty.net`  
**Transformation Rules:**
- Convert to lowercase
- Remove leading/trailing whitespace
- Validate email format with regex

---

### 7. address
**Description:** Raw physical address as found on website  
**Data Type:** String  
**Format:** Variable (as-scraped)  
**Required:** No (conditional)  
**Example:** `202 Mira Loma Drive, Oroville, CA 95965`  
**Notes:** May include street, city, state, ZIP

---

### 8. address_clean
**Description:** Standardized physical address  
**Data Type:** String  
**Format:** Normalized abbreviations  
**Required:** No (conditional)  
**Example:** `202 Mira Loma Drive, Oroville, CA 95965`  
**Transformation Rules:**
- Expand common abbreviations (St→Street, Ave→Avenue, Rd→Road)
- Remove extra whitespace
- Preserve original structure

---

### 9. type
**Description:** Type of resource record  
**Data Type:** String (Enum)  
**Format:** Controlled vocabulary  
**Required:** Yes  
**Allowed Values:**
- `phone` - Phone number record
- `email` - Email address record
- `address` - Physical address record  
**Example:** `phone`  
**Notes:** Used for record-level classification

---

### 10. category
**Description:** Primary category classification  
**Data Type:** String (Enum)  
**Format:** Controlled vocabulary  
**Required:** Yes  
**Allowed Values:**
- `CONTACT_INFO` - Phone numbers, emails, fax
- `LOCATION` - Addresses, geographic data
- `FACILITY` - Clinics, hospitals, health centers
- `SERVICE` - Health services offered  
**Example:** `CONTACT_INFO`  
**Classification Logic:**
- If phone or email present → CONTACT_INFO
- If address present → LOCATION
- If facility keywords in title → FACILITY
- Otherwise → SERVICE

---

### 11. health_topics
**Description:** Health topics identified in the resource  
**Data Type:** List of Strings  
**Format:** Array of keywords  
**Required:** No  
**Possible Values:**
- `vaccination` - Vaccines, immunization
- `covid` - COVID-19 related services
- `flu` - Flu shots, seasonal influenza
- `mental_health` - Mental health services
- `dental` - Dental care
- `pediatric` - Children's health
- `maternal` - Pregnancy, prenatal care
- `emergency` - Emergency services, crisis hotlines
- `chronic_disease` - Diabetes, hypertension management
- `substance_abuse` - Addiction recovery services  
**Example:** `["vaccination", "covid"]`  
**Detection Method:** Keyword matching in facility name and services text

---

### 12. scraped_at
**Description:** Timestamp when data was collected  
**Data Type:** String (ISO 8601 DateTime)  
**Format:** `YYYY-MM-DDTHH:MM:SS`  
**Required:** Yes  
**Example:** `2024-12-02T14:04:21.438358`  
**Timezone:** Local time (PST/PDT)  
**Notes:** Used for data freshness tracking

---

### 13. processed_at
**Description:** Timestamp when data was processed and cleaned  
**Data Type:** String (ISO 8601 DateTime)  
**Format:** `YYYY-MM-DDTHH:MM:SS`  
**Required:** Yes  
**Example:** `2024-12-02T14:04:21.438358`  
**Timezone:** Local time (PST/PDT)  
**Notes:** Indicates when cleaning/categorization occurred

---

## Data Relationships

### One-to-Many Structure
- Each `source_url` can have multiple records (phone, email, address)
- Each record has a unique combination of `source_url` + `type` + contact field

### Example:

source_url: https://example.com
├── Record 1: type=phone, phone_clean=(530) 552-3800
├── Record 2: type=email, email_clean=contact@example.com
└── Record 3: type=address, address_clean=123 Main St
