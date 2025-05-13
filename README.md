# Corporate Messenger System

A modern corporate messaging system built with microservices architecture, featuring real-time communication, authentication, and secure message delivery.

## 🔧 Technologies Used

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak-EDF2F7?style=for-the-badge&logo=keycloak&logoColor=black)
![Centrifugo](https://img.shields.io/badge/Centrifugo-00A4EF?style=for-the-badge&logo=centrifugo&logoColor=white)
![Yoyo](https://img.shields.io/badge/Yoyo-00A4EF?style=for-the-badge&logo=yoyo&logoColor=white)

</div>

## 🚀 Features

- Real-time messaging using Centrifugo
- Secure authentication with Keycloak
- Microservices architecture
- Scalable PostgreSQL databases
- Nginx reverse proxy
- Docker containerization

## 📋 Prerequisites

- Docker and Docker Compose
- Git
- Poetry (for local backend development)

## 🔧 Environment Setup

1. Create a `.env` file in the root directory with the following variables:

```env
KC_DB_USER=your_keycloak_db_user
KC_DB_PASS=your_keycloak_db_password
KC_DB_NAME=your_keycloak_db_name
KC_ADMIN_USER=your_keycloak_admin_user
KC_ADMIN_PASS=your_keycloak_admin_password
CENTRIFUGO_ADMIN_PASSWORD=your_centrifugo_admin_password
```

## 🚀 Getting Started

1. Clone the repository:
```bash
git clone <repository-url>
cd corporate_messenger_system
```

2. Start the services:
```bash
docker-compose up -d
```

3. Access the services:
- Frontend: http://localhost:8080
- Backend API: http://localhost:8001
- Keycloak Admin Console: http://localhost:7070
- Centrifugo Admin: http://localhost:8000

## 📊 Database Initialization and Testing

After setting up the services, you can initialize the database with test data. Here's how to do it:

1. Connect to the backend database:
```bash
docker exec -it corporate_messenger_system-backend_db-1 psql -U test -d test
```

2. Run the following SQL commands to create test channels and set up user permissions:

```sql
-- Create test channels
INSERT INTO public.channel (channel, title, "default") VALUES
('general', 'Общий канал', true),
('bob', 'Канал Боба', false),
('random', 'Случайный канал', false);

-- Set up Bob's permissions (replace with your user ID)
INSERT INTO public.user_channel (user_id, chan_id, can_publish, can_subscribe)
SELECT 'YOUR_BOB_USER_ID', id, true, true 
FROM public.channel 
WHERE channel IN ('general', 'bob');

-- Set up Alice's permissions (replace with your user ID)
INSERT INTO public.user_channel (user_id, chan_id, can_publish, can_subscribe)
SELECT 'YOUR_ALICE_USER_ID', id, false, true 
FROM public.channel 
WHERE channel IN ('general', 'random');
```

3. Verify the setup with these queries:
```sql
-- Check users
SELECT * FROM "user" u;

-- Check channels
SELECT * FROM channel;

-- Check user-channel permissions
SELECT * FROM user_channel;
```

Note: Replace `YOUR_BOB_USER_ID` and `YOUR_ALICE_USER_ID` with actual user IDs from your Keycloak instance. You can find these IDs in the Keycloak admin console under Users.

## 🏗️ Project Structure

```
corporate_messenger_system/
├── backend_service/     # Python backend service
├── frontend_service/    # Frontend application
├── protofiles/         # Protocol buffer definitions
├── configs/            # Configuration files
│   ├── nginx/         # Nginx configuration
│   └── centrifugo/    # Centrifugo configuration
└── docker-compose.yml  # Docker services configuration
```

## 🔄 Services

- **Frontend Service**: Web interface for the messaging system
- **Backend Service**: Python-based API service
- **Keycloak**: Authentication and authorization
- **Centrifugo**: Real-time messaging
- **PostgreSQL**: Database for both Keycloak and backend
- **Nginx**: Reverse proxy and static file serving

## 🛠️ Development

### Backend Development

1. Navigate to the backend directory:
```bash
cd backend_service
```

2. Install dependencies:
```bash
poetry install
```

3. Run migrations:
```bash
poetry run yoyo apply -b
```

4. Start the service:
```bash
poetry run python -m src
```

### Frontend Development

The frontend service is served through Nginx. Any changes to the files in `frontend_service/` will be automatically reflected.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request
