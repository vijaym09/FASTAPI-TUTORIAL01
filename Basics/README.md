# Day 01 — FastAPI Basics

Today I started learning FastAPI. I focused on setting up the
environment, creating a basic FastAPI application, creating GET
routes, running the server, and understanding Swagger documentation.

## Topics Covered

- FastAPI setup
- Virtual environment
- FastAPI application
- GET routes
- Uvicorn
- Python type hints
- Swagger UI
- ReDoc

## 1. Setting Up FastAPI

First, I created a virtual environment for the project:

```bash
python -m venv venv-fastapi

Then I installed FastAPI and Uvicorn:
pip install fastapi
pip install "uvicorn[standard]"

2. My First FastAPI Application

I created a main.py file:

from fastapi import FastAPI

app = FastAPI()

FastAPI() creates the main application object. I can use this
object to create different API routes.

3. Creating GET Routes

My first route:

@app.get("/")
async def index():
    return {"hello": "world"}

I also created an /about route:

@app.get("/about")
async def about():
    return "An exceptional company"