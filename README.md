# Frontend repo: https://github.com/eng-hussein-saad/ITI-Attendance-Frontend
# ITI Attendance & Lost/Found Backend

This is the backend system for the ITI Attendance Management and Lost & Found application. It provides APIs for managing student attendance, schedules, user authentication, and a system for reporting and matching lost and found items.

## Features

- **Attendance Management:**
  - Track student attendance based on schedules and sessions.
  - Geofencing for attendance validation (based on branch location).
  - Manual attendance marking by supervisors.
  - Attendance status tracking (attended, absent, late, excused).
  - Permission requests (early leave, late check-in, day excuse).
  - Absence warnings based on configurable thresholds.
  - Attendance statistics and trends reporting.
- **Lost & Found System:**
  - Report lost items with descriptions and images.
  - Report found items with descriptions and images.
  - AI-powered matching of lost and found items based on descriptions.
- **Real-time Notifications (via WebSockets):**
  - Notify users when a potential match is found for their lost/found item.
  - Notify students when their schedule is updated.
  - Notify supervisors when a student submits a permission request.
  - Notify the other user involved when a match is confirmed or declined.
- **User Management:**
  - User registration and authentication (JWT-based).
  - Role-based access control (Student, Supervisor, Admin).
  - User profile management.
- **Scheduling:**
  - Manage tracks, branches, and schedules.
  - Define daily sessions with instructors and types (online/offline).
- **API Documentation:**
  - Automatic API documentation using Swagger UI and Redoc (via drf-spectacular).

## Tech Stack

- **Backend Framework:** Django
- **API:** Django REST Framework (DRF)
- **Database:** PostgreSQL (Configured for Neon DB)
- **Authentication:** Djoser, DRF Simple JWT
- **Real-time:** Django Channels
- **Containerization:** Docker, Docker Compose
- **Dependency Management:** Pipenv
- **ML/AI:** Sentence Transformers (for lost/found matching), OpenCV (for image processing)
- **API Schema/Docs:** drf-spectacular

## Setup Instructions

### Prerequisites

- Python 3.11+
- Pipenv (`pip install pipenv`)
- Docker & Docker Compose (Optional, for containerized deployment)
- Git

### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd ITI-Attendance-Backend
```

### 2. Set Up Environment Variables

Create a `.env` file in the project root directory. You can copy the structure from `.env.example` (if you create one) and fill in your specific values. Key variables include:

```dotenv
# Django Settings
SECRET_KEY=your_django_secret_key
DEBUG=True # Set to False in production
ALLOWED_HOSTS=localhost,127.0.0.1 # Add your production host if needed

# Database (Neon Example)
DATABASE_URL=postgres://user:password@ep-project-host.us-east-1.neon.tech/dbname?sslmode=require&options=endpoint%3Dep-project-host
DATABASE_NAME=dbname # Optional, can be inferred from DATABASE_URL

# JWT Settings (Optional - defaults are usually fine)
# SIMPLE_JWT_ACCESS_TOKEN_LIFETIME_MINUTES=60
# SIMPLE_JWT_REFRESH_TOKEN_LIFETIME_DAYS=1

# Email Settings (If using email features like password reset)
# EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
# EMAIL_HOST=smtp.example.com
# EMAIL_PORT=587
# EMAIL_USE_TLS=True
# EMAIL_HOST_USER=your-email@example.com
# EMAIL_HOST_PASSWORD=your-email-password

# Frontend URL (For email links)
FRONTEND_BASE_URL=http://localhost:3000 # Or your frontend URL

# Channels (If using Redis)
# CHANNEL_LAYERS_BACKEND=channels_redis.core.RedisChannelLayer
# CHANNEL_LAYERS_HOST=127.0.0.1
# CHANNEL_LAYERS_PORT=6379
```

_Note: Ensure the `DATABASE_URL` includes the `options=endpoint%3D<your-neon-endpoint-id>` parameter as required by Neon._

### 3. Install Dependencies

Use Pipenv to install the required Python packages from the `Pipfile`.

```bash
pipenv install --dev
```

_(The `--dev` flag installs development dependencies as well.)_

### 4. Apply Database Migrations

Activate the virtual environment created by Pipenv and run Django migrations.

```bash
pipenv shell
python manage.py migrate
```

### 5. Create Superuser (Optional)

To access the Django admin interface:

```bash
python manage.py createsuperuser
```

## Running the Application

### Locally (using Pipenv and Uvicorn)

Ensure you are in the Pipenv shell (`pipenv shell`).

```bash
# Run using Uvicorn (recommended for ASGI features like Channels)
uvicorn core.asgi:application --host 0.0.0.0 --port 8000 --reload
```

_(The `--reload` flag automatically restarts the server when code changes.)_

### Using Docker

Ensure Docker is running.

1.  **Build the Docker image:**

    ```bash
    docker build -t iti-attendance-backend .
    ```

2.  **Run the Docker container:**

    ```bash
    docker run -p 8000:8000 --env-file .env iti-attendance-backend
    ```

    _(This maps port 8000 on your host to port 8000 in the container and loads environment variables from your `.env` file.)_

## API Documentation

Once the server is running, you can access the API documentation:

- **Swagger UI:** `http://localhost:8000/api/v1/schema-ui/`
- **Redoc:** `http://localhost:8000/api/v1/schema/redoc/`

## Management Commands

- **Generate Test Data:** Creates realistic mock data for development and testing.
  ```bash
  pipenv shell
  python manage.py generate_test_data
  ```
- **Generate Mock Data (Simpler):** Creates basic mock data.
  ```bash
  pipenv shell
  python manage.py generate_mock_data
  ```

## Contributors

- Omar Hany
- Hussein Saad
- Menna Reda
- Hoda Magdy
