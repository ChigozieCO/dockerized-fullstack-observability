# Base image for the backend
FROM python:3.10-slim AS builder

# Install dependencies
RUN apt update && apt install -y curl && apt clean

# Install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add poetry to PATH
ENV PATH="/root/.local/bin:$PATH"

# Set working directory and copy backend files
WORKDIR /backend
COPY pyproject.toml poetry.lock ./

# Install dependencies with Poetry
RUN poetry config virtualenvs.create true && poetry install --only main

# Second stage: Slim final image
FROM python:3.10-slim

# Set working directory and copy backend files
WORKDIR /backend

# Copy virtual environment from builder stage
COPY --from=builder /root/.cache/pypoetry/virtualenvs /venv

# Set virtual environment path
ENV PATH="/venv/bin:$PATH"

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add poetry to PATH
ENV PATH="/root/.local/bin:$PATH"

# Copy application files
COPY . .

# Expose application port
EXPOSE 8000

# Command to start the application
CMD ["poetry", "run", "uvicorn", "app.main:app", "--reload", "--", "--host"]