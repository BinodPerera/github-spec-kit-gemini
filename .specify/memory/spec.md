# Spec: Users Module

## Overview
This module handles user management within the FastAPI modular monolith.

## Requirements
1. Create a new user with email and password.
2. Retrieve user details by ID.
3. List all users.

## Architectural Design
Following the Constitution:
- **Router:** `app/modules/users/router.py` will handle HTTP requests.
- **Service:** `app/modules/users/service.py` will contain business logic.
- **Schemas:** `app/modules/users/schemas.py` will define Pydantic models for request/response.

## Data Structure (User)
- `id`: UUID (Primary Key)
- `email`: String (Unique)
- `hashed_password`: String
- `is_active`: Boolean (Default: True)

## Logic Flow
1. User sends POST request to `/users/`.
2. Router calls `UserService.create_user()`.
3. Service validates email uniqueness, hashes the password, and saves to DB.
4. Returns a `UserPublic` schema (excluding the password).