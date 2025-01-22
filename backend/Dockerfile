# Stage 1: Builder stage, Base image for the backend
FROM python:3.10-slim AS builder

# Install system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add poetry to PATH
ENV PATH="/root/.local/bin:$PATH"

# Set working directory and copy only dependency files first
WORKDIR /backend
COPY pyproject.toml poetry.lock* ./

# Ensure compatibility between Poetry and the lock file
RUN poetry self update 1.8.3

# Install dependencies with Poetry
RUN poetry config virtualenvs.create false && poetry install

# Stage 2: Slim final image
FROM python:3.10-slim

# Install system dependencies
RUN apt update && apt install -y \
    curl \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add poetry to PATH
ENV PATH="/root/.local/bin:$PATH"

# Set working directory
WORKDIR /backend

# Copy virtual environment from builder stage
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy application files
COPY . .

RUN poetry config virtualenvs.create false

# Expose application port
EXPOSE 8000

# Set the default command to run the application
CMD ["poetry", "run", "uvicorn", "app.main:app", "--reload", "--host", "0.0.0.0", "--port", "8000"]