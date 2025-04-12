![BilAuth Banner](./assets/banner.png)

---

# Bilauth

**Bilauth** is a Python module that enables programmatic access to Bilfen Lisesi's student portal. It provides tools for session-based authentication, HTML scraping, and structured parsing of student data like profiles, exams, and clubs.

---

## 📦 Installation

Install from PyPI:

```bash
pip install bilauth
```

---

## 📚 Features

- 🔐 **Context-managed session authentication**
- 📄 Fetch pages: profile, KKS exams, written exams, club selections
- 🧠 Parse HTML to structured Python dictionaries
- 🎯 High-level fetch + parse functions for quick integration

---

## 🚀 Example Usage

```python
from bilauth import Auth, get_profile_info, get_club_selections, get_written_exam_info, get_kks_exam_info
import requests

if __name__ == "__main__":
    # Replace with your actual TC and password
    tc_number = "YOUR_TC_NUMBER"
    password = "YOUR_PASSWORD"

    with Auth(tc_number, password) as auth:
        if auth.verified:
            print("Authentication successful. Proceeding with protected actions...")
            try:
                profile_data = get_profile_info(auth.session)
                print("\n--- Profile Information ---")
                print(profile_data)

                club_data = get_club_selections(auth.session)
                print("\n--- Club Selections ---")
                print(club_data)

                written_exam_data = get_written_exam_info(auth.session)
                print("\n--- Written Exam Results ---")
                print(written_exam_data)

                kks_exam_data = get_kks_exam_info(auth.session)
                print("\n--- KKS Exam Results ---")
                print(kks_exam_data)

            except requests.exceptions.RequestException as e:
                print(f"\nAn error occurred during a data request after successful login: {e}")
            except Exception as e:
                print(f"\nAn unexpected error occurred after login: {e}")
                import traceback
                traceback.print_exc()

        else:
            print("\nAuthentication failed. Cannot proceed to fetch data.")
```

---

## 🔧 API Overview

### 🔐 `Auth` – Context Manager

```python
Auth(tc: str, password: str)
```

- Automatically logs in on context enter.
- Closes session on exit.
- Use `.verified` to check login status.

---

### 📄 Low-Level Fetchers

```python
get_profile_page(session)
get_club_page(session)
get_written_exam_page(session)
get_kks_exam_page(session)
```

- Fetch raw HTML from the student portal.

---

### 🧠 Parsing Functions

```python
parse_profile_info(html)
parse_club_info(html)
parse_written_exam(html)
parse_kks_exam(html)
```

- Convert HTML into structured Python dictionaries.

---

### 🎯 High-Level Helpers

```python
get_profile_info(session)
get_club_selections(session)
get_written_exam_info(session)
get_kks_exam_info(session)
```

- Combine fetching + parsing in one step.

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

1. Fork this repository
2. Create a new feature branch
3. Follow [PEP8](https://peps.python.org/pep-0008/) and add docstrings
4. Submit a PR

---

## 📄 License

Licensed under the **BILAUTH OPEN DECLARATION LICENSE (BODL)**. See the `LICENSE` file for full terms.

---

## 💬 Attribution

Built for and by the Bilfen student community.  
Designed by **Kağan Erkan**.

Want to show you’re compliant? Add the badge:

**Built with Bilauth 🛡️**
