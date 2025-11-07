# Web-Scraping-API-Task

Task 1: Web Scraping IBPS Job Listings
🎯 Objective:

Scrape job listings from the IBPS (Institute of Banking Personnel Selection) official recruitment page — https://www.ibps.in

✅ Extract:

Job Title

Location (if available)

Post/Publish Date

Link to Detailed Job Page

📦 Output:

Save as a CSV file: ibps_jobs.csv

Step-by-step Code (Python)
import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL of IBPS recruitment page
URL = "https://www.ibps.in/recruitment-process/recruitment-notifications/"

# Fetch page content
response = requests.get(URL)
soup = BeautifulSoup(response.text, 'html.parser')

# Find job listings
jobs = []

# Each notification is inside <li> elements under class 'elementor-icon-list-item'
for li in soup.select('li.elementor-icon-list-item'):
    title_tag = li.find('span', class_='elementor-icon-list-text')
    link_tag = li.find('a')

    if title_tag and link_tag:
        job_title = title_tag.text.strip()
        job_link = link_tag['href']

        # Some job links may contain date or job code in title
        post_date = ''
        if any(char.isdigit() for char in job_title):
            post_date = ''.join(filter(str.isdigit, job_title))  # crude extract if date exists

        jobs.append({
            'Job Title': job_title,
            'Location': 'India',  # IBPS is India-based, location rarely mentioned
            'Post/Publish Date': post_date if post_date else 'N/A',
            'Job Link': job_link
        })

# Convert to DataFrame and save as CSV
df = pd.DataFrame(jobs)
df.to_csv('ibps_jobs.csv', index=False)

print("✅ Scraping completed. Saved as ibps_jobs.csv")
print(df.head())

🧪 Output Example:
| Job Title                               | Location | Post/Publish Date | Job Link                                        |
| --------------------------------------- | -------- | ----------------- | ----------------------------------------------- |
| Recruitment of Specialist Officers 2025 | India    | N/A               | [https://www.ibps.in/](https://www.ibps.in/)... |
| CRP Clerks-XIV Notification             | India    | N/A               | [https://www.ibps.in/](https://www.ibps.in/)... |





Task 2: Django REST API (Login Authentication)
🎯 Objective:

Create a minimal Django REST Framework API with JWT or DRF Token Authentication.

⚙️ Project Setup Commands
# Create project & app
django-admin startproject ibps_auth
cd ibps_auth
python manage.py startapp api

# Install dependencies
pip install djangorestframework djangorestframework-simplejwt

# Add to settings.py
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',
    'api',
]

# Configure REST Framework
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    )
}

🧩 api/views.py

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from django.contrib.auth import authenticate
from rest_framework_simplejwt.tokens import RefreshToken

class LoginView(APIView):
    def post(self, request):
        username = request.data.get("username")
        password = request.data.get("password")

        user = authenticate(username=username, password=password)
        if user:
            refresh = RefreshToken.for_user(user)
            return Response({
                "refresh": str(refresh),
                "access": str(refresh.access_token)
            }, status=status.HTTP_200_OK)
        return Response({"error": "Invalid credentials"}, status=status.HTTP_401_UNAUTHORIZED)

🧾 api/urls.py

from django.urls import path
from .views import LoginView

urlpatterns = [
    path('login/', LoginView.as_view(), name='login'),
]


🌐 ibps_auth/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')),
]


👤 Create Test User

python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser --username=testuser --email=test@example.com
# Password: testpass123

🧪 Test the API

POST → /api/login/

Request Body (JSON)
{
  "username": "testuser",
  "password": "testpass123"
}
{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "access": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}


✅ Test Command (with curl)

curl -X POST http://127.0.0.1:8000/api/login/ \
     -H "Content-Type: application/json" \
     -d '{"username":"testuser","password":"testpass123"}'
