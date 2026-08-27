# Agile Estimation System

A real-time **Agile Estimation / Planning Poker** web application built for Scrum teams to collaboratively estimate user stories and tickets.

The system allows a **Moderator** to create estimation sessions, manage tickets, control voting, reveal estimates, and finalize story points. **Developers** and **Testers** can join sessions and submit estimates in real time.

## Features

* User registration and login
* Role-based access: Moderator, Developer, Tester
* JWT authentication with refresh tokens
* Create and join estimation sessions
* Unique session codes
* Create, update, delete, and activate tickets
* Real-time Planning Poker using SignalR
* Hidden voting until votes are revealed
* Developer and Tester vote segregation
* Reset votes and re-estimate tickets
* Finalize ticket estimates
* Persistent session, ticket, and voting data
* API validation and centralized error handling
* Swagger API documentation
* Structured logging using Serilog
* SQL Server health check

## Tech Stack

### Frontend

* React 19
* TypeScript
* Vite
* React Router
* TanStack React Query
* Zustand
* Axios
* SignalR Client
* React Hook Form
* Zod
* Tailwind CSS

### Backend

* ASP.NET Core Web API
* .NET 10
* SignalR
* Entity Framework Core
* SQL Server
* JWT Authentication
* BCrypt
* FluentValidation
* AutoMapper
* Serilog
* Swagger / OpenAPI

## Architecture

```text
React Frontend
      ↓
REST API / SignalR
      ↓
ASP.NET Core Controllers / Hub
      ↓
Services
      ↓
Repositories
      ↓
Entity Framework Core
      ↓
SQL Server
```

The backend follows a layered architecture:

```text
AgileEstimation.API
AgileEstimation.Application
AgileEstimation.Domain
AgileEstimation.Infrastructure
AgileEstimation.Persistence
```

## Application Workflow

```text
Register / Login
      ↓
Moderator creates a session
      ↓
Developer / Tester joins using session code
      ↓
Moderator creates and activates a ticket
      ↓
Participants submit hidden votes
      ↓
Moderator reveals votes
      ↓
Team discusses estimates
      ↓
Moderator finalizes the estimate
```

## User Roles

### Moderator

* Create estimation sessions
* Manage tickets
* Start estimation
* Reveal and reset votes
* Finalize estimates
* Close sessions

### Developer

* Join sessions
* Vote on active tickets
* View Developer-side revealed estimates

### Tester

* Join sessions
* Vote on active tickets
* View Tester-side revealed estimates

## Planning Poker Cards

```text
0, 1, 2, 3, 5, 8, 13, 21, ?, ☕
```

`?` and `☕` are special cards and are not included in numeric average calculations.

## Real-Time Communication

SignalR is used for live collaboration.

Hub endpoint:

```text
/hubs/planning-poker
```

Main real-time operations include:

* Join session
* Leave session
* Cast vote
* Reveal votes
* Reset votes
* Finalize estimate
* Participant updates
* Ticket activation
* Session closing

## Database

Main entities:

```text
User
Session
SessionParticipant
Ticket
Vote
RefreshToken
```

Entity Framework Core is used as the ORM and SQL Server is used as the relational database.

## Authentication

The application uses JWT-based authentication.

```text
Login
  ↓
Access Token + Refresh Token
  ↓
Authenticated API Requests
  ↓
Access Token Expires
  ↓
Refresh Token Rotation
  ↓
New Access Token + Refresh Token
```

Passwords are hashed using BCrypt, and refresh tokens are hashed before being stored in the database.

## Getting Started

### Prerequisites

Make sure you have installed:

* .NET 10 SDK
* Node.js
* npm
* SQL Server
* Entity Framework Core CLI

### Backend Setup

Open the solution directory:

```bash
cd AgileEstimationSystem
```

Restore dependencies:

```bash
dotnet restore AgileEstimationSystem.slnx
```

Configure the JWT key:

```bash
dotnet user-secrets set "Jwt:Key" "your-long-development-secret-key" --project backend/AgileEstimation.API
```

Apply database migrations:

```bash
dotnet ef database update --project backend/AgileEstimation.Persistence --startup-project backend/AgileEstimation.API
```

Run the backend:

```bash
dotnet run --project backend/AgileEstimation.API
```

Backend URL:

```text
https://localhost:7248
```

Swagger:

```text
https://localhost:7248/swagger
```

## Frontend Setup

Move to the frontend directory:

```bash
cd frontend
```

Install dependencies:

```bash
npm install
```

Configure `.env`:

```env
VITE_API_BASE_URL=https://localhost:7248
VITE_SIGNALR_HUB_URL=https://localhost:7248/hubs/planning-poker
```

Start the frontend:

```bash
npm run dev
```

Frontend URL:

```text
http://localhost:5173
```

## Project Structure

```text
AgileEstimationSystem/
├── backend/
│   ├── AgileEstimation.API
│   ├── AgileEstimation.Application
│   ├── AgileEstimation.Domain
│   ├── AgileEstimation.Infrastructure
│   └── AgileEstimation.Persistence
│
└── frontend/
    └── src/
        ├── api
        ├── components
        ├── hooks
        ├── pages
        ├── routes
        ├── signalr
        └── store
```

## Summary

The **Agile Estimation System** provides a complete real-time Planning Poker workflow with role-based collaboration, secure authentication, session and ticket management, persistent voting data, and SignalR-based live updates.

It demonstrates full-stack development using **React, TypeScript, ASP.NET Core, SignalR, Entity Framework Core, and SQL Server**.
