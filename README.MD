# Bilauth

**Bilauth** is a Python module that enables programmatic access to Bilfen Lisesi's student portal, offering tools for authentication, data scraping, and structured parsing of profile, exam, and club information. Designed for Bilfen students and developers, Bilauth powers student-only tools, early-stage app testing, and competition-specific access scenarios.

---

## 📦 Installation

Install directly from PyPI:

```bash
pip install bilauth
```

---

## 📚 Features

- 🔐 **Session-based authentication** with a context manager
- 📄 Fetch pages: profile, KKS results, written exams, club selections
- 🧠 Parse HTML into structured dictionaries
- 🧰 High-level data extraction for easy use in apps and interfaces

---

## 🚀 Quick Start

```python
from bilauth import Auth, get_profile_info, get_club_selections, get_written_exam_info, get_kks_exam_info

with Auth(tc="your_tc", password="your_password") as session:
    print(get_profile_info(session))
    print(get_club_selections(session))
    print(get_written_exam_info(session))
    print(get_kks_exam_info(session))
```

---

## 🔧 Functions Overview

### 🔐 Auth Context Manager
```python
Auth(tc: str, password: str)
```
- Logs in on enter, closes session on exit.

### 📄 Fetch Functions
```python
get_profile_page(session)
get_club_page(session)
get_kks_exam_page(session)
get_written_exam_page(session)
```
- Low-level HTML fetchers.

### 🧠 Parsing Functions
```python
parse_profile_info(html)
parse_club_info(html)
parse_written_exam(html)
parse_kks_exam(html)
```
- Turn HTML into usable Python dictionaries.

### 🎯 High-Level Helpers
```python
get_profile_info(session)
get_club_selections(session)
get_written_exam_info(session)
get_kks_exam_info(session)
```
- Fetch + parse combined.

---

## 🛠 Dependencies

- `requests`
- `beautifulsoup4`

Install manually if needed:
```bash
pip install requests beautifulsoup4
```

---

## 🤝 Contributing

1. Fork the repo
2. Create your feature branch
3. Write clear docstrings and follow [PEP8](https://peps.python.org/pep-0008/)
4. Submit a PR

---

## 📄 License

Licensed under the **BILAUTH OPEN DECLARATION LICENSE (BODL)**. See `LICENSE` for full terms.

---

## 💬 Attribution

Built for and by the Bilfen student community. Designed by Kağan Erkan.

Want to show you're compliant? Use the badge:

**Built with Bilauth 🛡️**

