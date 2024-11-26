# Base image for the backend
FROM python:3.10-slim

# Install dependencies
RUN apt update && apt install -y curl && apt clean

# Install poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Add poetry to PATH
ENV PATH="$HOME/.local/bin:$PATH"

# Set working directory and copy backend files
WORKDIR /backend
COPY . /backend

# Navigate into the backend directory
RUN cd backend

# Install dependencies with Poetry
RUN poetry config virtualenvs.create false && poetry install --no-root
