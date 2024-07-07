FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Copy the dependency files to the working directory
COPY pyproject.toml poetry.lock ./

# Install Poetry
RUN pip install poetry

# Install dependencies
RUN poetry install --no-dev

# Copy the rest of the application code
COPY . .

# Expose the port that your application runs on
EXPOSE 8000

# Convert line endings of prestart.sh to Unix format
RUN apt-get update && apt-get install -y dos2unix \
    && dos2unix ./prestart.sh \
    && chmod +x ./prestart.sh

# Run the database initialization script
ENV PYTHONPATH=/app

# Command to run the application
CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]

