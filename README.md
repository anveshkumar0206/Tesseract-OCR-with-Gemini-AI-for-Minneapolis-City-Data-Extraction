# Minneapolis 1900 City Directory Data Extraction

This project extracts structured resident and business information from scanned pages of the 1900 Minneapolis City Directory using Google Gemini's vision model and post-processing techniques. It aims to build a clean, JSON-formatted dataset from historical OCR-unfriendly documents.

## Objective

Digitize and extract the following structured fields for each resident or business listed in the city directory:
- First Name, Last Name (with middle initials)
- Spouse Name (if listed)
- Occupation and Company Name
- Home Address (number, street, apartment/unit, residence indicator)
- Work Address (if present)
- Telephone (rarely listed)
- Metadata: Directory Name and Page Number

## Key Features

- **Vision AI Integration:** Uses Gemini Flash (Multimodal) to understand and extract data from historical images
- **Abbreviation Expansion:** Dynamically detects and expands abbreviations using a pre-parsed dictionary from the abbreviation page (e.g., `slsmn` → `salesman`)
- **Smart Parsing Logic:** Fixes OCR quirks such as:
  - Middle initials (e.g., "Oscar S." as first name, not first + last)
  - Avoids carry-over of last names between entries
  - Cleans extraneous company address descriptors
- **Batch Processing:** Supports processing multiple PDF pages (e.g., 1900_page112–116)

## Project Structure

├── abbreviations.json # Extracted abbreviation dictionary
├── structured_directory_output.json # Final combined structured output
├── 1900_page112.pdf, 1900_page113.pdf, 1900_page114.pdf, 1900_page115.pdf, 1900_page116.pdf # Input scanned directory PDFs
├── Spot_Check.ipynb # Main notebook or script
├── README.md # Project documentation

## Technologies Used

- Google Gemini API (`gemini-2.0-flash`)
- Python 3 (with `pdf2image`, `Pillow`, `json`, `re`)
- Google Colab environment (via `userdata` for secure API usage)
- Tesseract for OCR pipelines

## How to Run

1. Upload the abbreviation and target pages (e.g., `1900_page111.pdf`, `1900_page112.pdf`, etc.)
2. Store your Gemini API Key in Colab using:

```python
from google.colab import userdata
userdata.set("GOOGLE_API_KEY", "your-api-key")
Run the notebook to generate the JSON output.

Example Output
json

{
  "FirstName": "Oscar S.",
  "LastName": null,
  "Spouse": null,
  "Occupation": "Bartender",
  "CompanyName": null,
  "HomeAddress": {
    "StreetNumber": "242",
    "StreetName": "20th av n",
    "ApartmentOrUnit": null,
    "ResidenceIndicator": "r"
  },
  "WorkAddress": null,
  "Telephone": null,
  "DirectoryName": "Minneapolis 1900",
  "PageNumber": 112
}
