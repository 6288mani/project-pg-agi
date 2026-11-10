# Stage 1: Builder
FROM python:3.9-slim AS builder

WORKDIR /app

# Prevent Python from writing .pyc files & buffer stdout/stderr
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Install dependencies into a virtual environment
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt


# Stage 2: Runtime
FROM python:3.9-slim AS runtime

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PATH="/opt/venv/bin:$PATH"

# Copy pre-built virtual environment from builder stage
COPY --from=builder /opt/venv /opt/venv

# Copy remaining application source code
COPY . .

EXPOSE 8000


# Start Uvicorn bound to 0.0.0.0 so external traffic can reach it inside Docker
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]