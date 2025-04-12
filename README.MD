# Bilauth Module Documentation

The **Bilauth** module provides functionality to authenticate against the Bilfen Lisesi’s online portal and fetch various pages (profile, exam results, club selections) with automated parsing of the resulting HTML. This module is designed as a single file (`bilauth.py`) that includes both low-level and high-level functions to interact with the website, retrieve data, and extract structured information using HTML parsing. It was created to empower Bilfen students to design exclusive tools and interfaces tailored for other Bilfen students. It is also intended to serve as a temporary authentication system during early-stage (alpha/beta) software development or within competitions where quick, secure, student-only access is required.

## Table of Contents

- [Overview](#overview)
- [Installation Requirements](#installation-requirements)
- [Usage](#usage)
  - [Authentication Context Manager](#authentication-context-manager)
  - [Fetching Pages](#fetching-pages)
  - [Parsing Functions](#parsing-functions)
  - [High-Level Functions](#high-level-functions)
- [Detailed Function Reference](#detailed-function-reference)
  - [Class: Auth](#class-auth)
  - [Functions to Fetch HTML Pages](#functions-to-fetch-html-pages)
  - [Parsing Functions](#parsing-functions-details)
  - [High-Level Data Extraction Functions](#high-level-data-extraction-functions)
- [Example Usage](#example-usage)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The **Bilauth** module performs the following tasks:

1. **Authentication:**  
   It uses a context manager (`Auth`) to manage a session-based login to the Bilfen Lisesi website. During initialization, the module logs in using provided credentials.

2. **Fetching Data:**  
   Several functions fetch HTML pages such as:
   - KKS exam results page
   - Profile page (which includes student and parents’ data)
   - Club selections page
   - Written exam results page

3. **Parsing Data:**  
   After fetching the HTML pages, the module employs BeautifulSoup to parse the HTML and extract structured data:
   - **Profile information** is extracted from tables into dictionaries.
   - **Club selections** are parsed into a dictionary mapping each selection preference (e.g., "Tercih 1") to its chosen club.
   - **Written and KKS exam results** are parsed from tabbed interfaces into dictionaries with term-wise exam scores.

4. **High-Level Functions:**  
   The module offers high-level helper functions which fetch and parse the relevant pages, returning structured data that can be directly used in other parts of an application or for report generation.

---

## Installation Requirements

Make sure you have the following Python packages installed:

- **requests**: For handling HTTP requests.
- **beautifulsoup4**: For parsing HTML content.

You can install the dependencies using pip:

```bash
pip install requests beautifulsoup4
```

---

## Usage

### Authentication Context Manager

The `Auth` class is a context manager that creates and manages a persistent session for the website. When entering the context, it automatically logs in using the provided login URL, credentials, and headers. On exit, it cleanly closes the session.

### Fetching Pages

The module defines several functions to fetch specific web pages:
- `get_kks_exam_page(session)`: Retrieves the raw HTML for the KKS exam results.
- `get_profile_page(session)`: Retrieves the user profile page.
- `get_club_page(session)`: Retrieves the club selections page.
- `get_written_exam_page(session)`: Retrieves the written exam results page.

All functions use the session from the `Auth` context manager and include error-handling using `raise_for_status`.

### Parsing Functions

For each type of page, there are dedicated parsing functions:

- `parse_profile_info(html)`: Extracts the student and parents’ data from the profile page.  
- `parse_club_info(html)`: Extracts club preference selections from the club page.
- `parse_written_exam(html)`: Extracts term-based exam scores from the written exam results page.
- `parse_kks_exam(html)`: Extracts term-based exam scores from the KKS exam results page.

Each parser uses BeautifulSoup to locate the relevant HTML elements (tables, rows, etc.) and then processes the data into dictionaries for easier consumption.

### High-Level Functions

The following functions combine fetching and parsing:
- `get_profile_info(session) -> dict`: Returns parsed student and parent information.
  - Uses `get_profile_page()` and `parse_profile_info()`.
- `get_club_selections(session) -> dict`: Returns the club selections as a dictionary.
  - Uses `get_club_page()` and `parse_club_info()`.
- `get_written_exam_info(session) -> dict`: Returns structured exam data from the written exam page.
  - Uses `get_written_exam_page()` and `parse_written_exam()`.
- `get_kks_exam_info(session) -> dict`: Returns structured exam data from the KKS exam page.
  - Uses `get_kks_exam_page()` and `parse_kks_exam()`.

---

## Detailed Function Reference

### Class: Auth

- **Initialization (`__init__`)**
  - **Parameters:**
    - `username (str)`: The username for login.
    - `password (str)`: The password for login.
    - `login_url (str, optional)`: Custom login URL (defaults to Bilfen’s login page).
    - `headers (dict, optional)`: Optional headers for the request.
  - Initializes login payload with credentials.

- **Enter (`__enter__`)**
  - Posts the login payload to the authentication URL.
  - Prints a success message upon successful login; otherwise prints the failure status.
  - Returns the authenticated session object.

- **Exit (`__exit__`)**
  - Closes the session when the context is exited.

### Functions to Fetch HTML Pages

- **get_kks_exam_page(session)**
  - URL: `https://bilgimerkezi.bilfenlisesi.com/sinav-sonuclari/konu-kavrama`

- **get_profile_page(session)**
  - URL: `https://bilgimerkezi.bilfenlisesi.com/profil`

- **get_club_page(session)**
  - URL: `https://bilgimerkezi.bilfenlisesi.com/kulup`

- **get_written_exam_page(session)**
  - URL: `https://bilgimerkezi.bilfenlisesi.com/sinav-sonuclari/yazili-sinav`

### Parsing Functions Details

- **parse_profile_info(html) -> dict**
  - Searches for panel headings containing "ÖĞRENCİ BİLGİLERİ".
  - Extracts student and parents’ data into a dictionary.

- **parse_club_info(html) -> dict**
  - Extracts preference-based club selections.
  - Returns an error dictionary if container is missing.

- **parse_written_exam(html) -> dict**
  - Parses tab-pane content by term.
  - Returns a dictionary of subjects and scores.

- **parse_kks_exam(html) -> dict**
  - Similar to written exam parser but for KKS.

---

## Example Usage

```python
#!/usr/bin/env python3
from bilauth import Auth, get_profile_info, get_club_selections, get_written_exam_info, get_kks_exam_info

def main():
    with Auth(username='your_username', password='your_password') as session:
        print("Profile Data:", get_profile_info(session))
        print("\nClub Selections:", get_club_selections(session))
        print("\nWritten Exam Results:", get_written_exam_info(session))
        print("\nKKS Exam Results:", get_kks_exam_info(session))

if __name__ == "__main__":
    main()
```

---

## Contributing

Bug reports, feature suggestions, or pull requests are welcome. Please:
- Use PEP8 style conventions.
- Include test cases if submitting code.
- Add docstrings if you create new public-facing functions.

To contribute:
```bash
git clone https://github.com/yourusername/bilauth
cd bilauth
pip install -r requirements.txt
```

---

## License

This project is licensed under the **BILAUTH OPEN DECLARATION LICENSE (BODL) v1.0**.  
See the [LICENSE file](./LICENSE) or read the full license terms below:

---

**BILAUTH OPEN DECLARATION LICENSE (BODL)**  
**Version 1.0 – [Year]**  
Copyright (c) [Year] [Your Name or Organization]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software (the “Software”) and associated documentation files (the “Documentation”), to deal in the Software without restriction—including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software and to permit persons to whom the Software is furnished to do so—subject to the following conditions:

[Full BODL license terms would be included here or in a separate LICENSE file.]

---