# 🧩 Full Stack Assignment — IBPS Jobs & Auth API  
**Author:** Yashwanth Royal Ande  
**Date:** November 2025  

---

## 📘 Overview  

This repository contains two main components for the full-stack coding assignment:

1. **Web Scraping Task** — Scrapes IBPS job listings and saves data to a CSV.  
2. **Django REST API Task** — Implements a secure login API using JWT authentication.  

Both tasks are built to demonstrate practical skills in **Python scripting**, **data extraction**, **REST API design**, and **authentication handling**.

---

## 🕸️ 1. Web Scraping — IBPS Job Listings  

### 🎯 Objective
Scrape job listings from the official **IBPS (Institute of Banking Personnel Selection)** recruitment page:  
🔗 [https://www.ibps.in/recruitment-process/recruitment-notifications/](https://www.ibps.in/recruitment-process/recruitment-notifications/)

### ✅ Extracted Fields
- Job Title  
- Location  
- Post/Publish Date  
- Link to Detailed Job Page  

### 🧠 Tech Used
- Python  
- `requests`  
- `beautifulsoup4`  
- `pandas`

### 📦 Output
Saves results into a CSV file: `ibps_jobs.csv`

### ▶️ Run Instructions
```bash
cd ibps_web_scraper
pip install -r requirements.txt
python ibps_scraper.py

📊 Sample Output
| Job Title                               | Location | Post/Publish Date | Job Link                                        |
| --------------------------------------- | -------- | ----------------- | ----------------------------------------------- |
| Recruitment of Specialist Officers 2025 | India    | N/A               | [https://www.ibps.in/](https://www.ibps.in/)... |
| CRP Clerks-XIV Notification             | India    | N/A               | [https://www.ibps.in/](https://www.ibps.in/)... |

⚙️ 2. Django REST API — Login Authentication
🎯 Objective

A simple REST API built using Django REST Framework and Simple JWT for authentication.

🧩 Endpoint

POST /api/login/

🧾 Request Example
{
  "username": "testuser",
  "password": "testpass123"
}
🔐 Response Example
{
  "refresh": "JWT_REFRESH_TOKEN",
  "access": "JWT_ACCESS_TOKEN"
}
🧱 Tech Stack

Django

Django REST Framework

SimpleJWT (for token authentication)

▶️ Run Instructions
cd ibps_auth_api
pip install -r requirements.txt
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser --username=testuser --email=test@example.com
# password: testpass123
python manage.py runserver

Test login at:
➡️ http://127.0.0.1:8000/api/login/

🧑‍💻 Test User Credentials
| Field        | Value         |
| ------------ | ------------- |
| **Username** | `testuser`    |
| **Password** | `testpass123` |

📝 Notes

Scraping uses only publicly available data from the official IBPS website.

JWT authentication ensures secure token-based login for API clients.

Code is modular, easy to extend, and production-ready for demonstration purposes.

🏁 Author

Yashwanth Royal Ande

