# Web_Project_02
# PawEver Pet Clinic Management System

A simple and user-friendly **Pet Clinic Management** web application built with **React + Vite (Frontend)** and **Express + MongoDB (Backend)**.  
This project helps pet owners, doctors, and staff manage appointments, pets, medical records, vaccination reminders, and billing in one system.

---
## Team Members

1. Kaung Myat Thu - u6712065@au.edu - KaungMyatThu30
2. Thant Sin Win - u6712095@au.edu - thantsinwin8917
3. Htin Aung Lynn - u6726116@au.edu - Lynn198456

---
## Features

### Page 1: Login / Role Access

Users can access the system by role:

- **Pet Owner**
- **Doctor**
- **Staff**
- **Forgot Password (token-based demo flow)**

---
### Login Screenshot

![Login ScreenShot](images/login.png)

---
### Page 2: Pet Owner Dashboard

Pet owners can manage pets and appointments:

- **Dashboard Overview**
  - Upcoming appointment
  - Vaccination reminders
  - Notifications
- **Book Appointment**
  - Doctor/date/time selection
  - Conflict checking (backend)
- **My Pets**
  - Add / edit / delete pet
  - Breed dropdown selection
  - Upload pet photo
- **Medical Records**
  - Visit history
  - Vaccination history timeline
  - Vaccination schedule engine (next due date, due soon / overdue)
- **Profile**
  - Contact preferences
  - Password change

---
### Pet Owner Dashboard Screenshot

![Pet Owner Screenshot](images/petowner-dashboard.png)

---
### Page 3: Doctor Dashboard

Doctors can manage consultations and care updates:

- View appointments
- Update appointment status
- Create medical records
- Add prescription and vaccine details
- Manage doctor schedule / availability

---
### Doctor Dashboard Screenshot

![Doctor Dashboard Screenshot](images/doctor-dashboard.png)

---
### Page 4: Staff Dashboard

Staff can manage operations:

- Appointment management
- Pet owner / doctor / user management
- Billing & Payments
  - Invoice number
  - Payment methods (**cash / card / transfer**)
  - Partial payment support
  - Unpaid balance tracking
  - Tax and discount fields
- Reports & analytics

---
### Staff Dashboard Screenshot

![Staff Dashboard Screenshot](images/staff-dashboard.png)

---
## Deploy Link

- Azure VM: [https://pawever.koreacentral.cloudapp.azure.com](https://pawever.koreacentral.cloudapp.azure.com)

---
## Technology Stack

- **Frontend:** React (Vite), React Router DOM
- **Backend:** Express.js, Node.js
- **Database:** MongoDB Atlas (Mongoose)
- **Authentication:** bcryptjs (password hashing)
- **Deployment:** Docker, Nginx, Azure VM
- **Language:** JavaScript (ES6+)

---
## Prerequisites

- Node.js (18+ recommended, 20 used in Docker)
- npm
- MongoDB Atlas connection string
- Docker (for deployment)

---
## Getting Started

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd Web_Project_02
```

### 2. Backend Setup

```bash
cd web_project_2_backend
npm install
```

Create `.env` or `.env.local`:

```env
PORT=5001
MONGODB_URI=mongodb+srv://Lynn198:1234@cluster0.xtk7ypf.mongodb.net/Web_Project_2?appName=Cluster0
PETS_MONGODB_URI=mongodb+srv://Lynn198:1234@cluster0.xtk7ypf.mongodb.net/Web_Project_2?appName=Cluster0
FRONTEND_URL=https://pawever.koreacentral.cloudapp.azure.com
```

Run backend:

```bash
npm run dev
```

### 3. Frontend Setup

```bash
cd ../web_project_2_frontend
npm install
```

Create `.env`:

```env
VITE_API_BASE_URL=https://pawever.koreacentral.cloudapp.azure.com
```

Run frontend:

```bash
npm run dev
```

Open:

- `https://pawever.koreacentral.cloudapp.azure.com`

### 4. Build for production

Frontend:

```bash
cd web_project_2_frontend
npm run build
```

Backend:

```bash
cd ../web_project_2_backend
npm run start
```

---
## Docker Deployment (Azure VM)

### Create Docker Network

```bash
sudo docker network create pawever-net || true
```

### Backend Image + Container

```bash
sudo docker build -t backend:1.0 ./web_project_2_backend
```

```bash
sudo docker run -d \
  --name web_project_2_backend \
  --network pawever-net \
  -p 5001:5001 \
  -e PORT=5001 \
  -e FRONTEND_URL=https://pawever.koreacentral.cloudapp.azure.com \
  -e MONGODB_URI='mongodb+srv://Lynn198:1234@cluster0.xtk7ypf.mongodb.net/Web_Project_2?appName=Cluster0' \
  -e PETS_MONGODB_URI='mongodb+srv://Lynn198:1234@cluster0.xtk7ypf.mongodb.net/Web_Project_2?appName=Cluster0' \
  backend:1.0
```

### Frontend Image + Container

```bash
sudo docker build -t frontend:1.0 ./web_project_2_frontend
```

```bash
sudo docker run -d \
  --name web_project_2_frontend \
  --network pawever-net \
  -p 80:80 \
  frontend:1.0
```

### Frontend Nginx Proxy (important)

`web_project_2_frontend/nginx.conf` must include:

```nginx
location /api/ {
  proxy_pass http://web_project_2_backend:5001;
  proxy_http_version 1.1;
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto $scheme;
}
```

Without this, login/API requests may return `405 Not Allowed`.

---
## Project Structure

```text

├── web_project_2_frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── parts/
│   │   │   ├── Login/
│   │   │   ├── PetOwner/
│   │   │   ├── Doctor/
│   │   │   └── Staff/
│   │   ├── lib/
│   │   ├── styles/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   ├── vite.config.js
│   ├── Dockerfile
│   └── nginx.conf
├── web_project_2_backend/
│   ├── src/
│   │   ├── config/
│   │   ├── controllers/
│   │   ├── lib/
│   │   ├── middleware/
│   │   ├── models/
│   │   ├── routes/
│   │   └── sever.js
│   ├── package.json
│   └── Dockerfile
```

---
## Application Specifications

### Appointment Management

- Prevents double-booking for doctor time slots
- Validates clinic hours / blocked slots
- Status flow:
  - `Pending -> Confirmed -> Completed / Cancelled`

### Vaccination Schedule Engine

- Auto-calculates next due date from vaccination records
- Reminder status:
  - `Due soon`
  - `Overdue`
  - `Up to date`
- Vaccine history timeline in Pet Owner medical records

### Billing & Payments

- Invoice number generation
- Tax / discount calculation
- Partial payment tracking
- Unpaid balance calculation
- Receipt generation and print view

---
## Scripts

### Frontend (`web_project_2_frontend`)

- `npm run dev` - Start Vite development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

### Backend (`web_project_2_backend`)

- `npm run dev` - Start backend with watch mode
- `npm run start` - Start backend server

---
## Troubleshooting

### `405 Not Allowed` on `/api/auth/login`

- Frontend Nginx is not proxying `/api`
- Fix `web_project_2_frontend/nginx.conf` and rebuild frontend image

### `host not found in upstream "web_project_2_backend"`

- Frontend and backend containers are not on the same Docker network
- Recreate both containers with `--network pawever-net`

### `MongoServerError: Authentication failed`

- MongoDB Atlas credentials are incorrect or password is not URL-encoded
- Reset DB user password and update container environment variables

### `ERR_CONNECTION_REFUSED`

- Frontend container is not running or port `80` is not bound
- Check `sudo docker ps` and `sudo docker logs web_project_2_frontend`

---
## License

This project is developed as part of a University course project by the team members listed above.

Built by Kaung Myat Thu, Thant Sin Win, Htin Aung Lynn.

