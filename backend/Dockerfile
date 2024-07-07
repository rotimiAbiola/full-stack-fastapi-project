FROM python:3.11.9

# Set environment variables
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

# Set working directory
WORKDIR /app

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -

# Ensure Poetry is available in the PATH
ENV PATH="${PATH}:/root/.local/bin"

# Copy the poetry.lock and pyproject.toml files
COPY poetry.lock pyproject.toml ./

# Disable virtual environment creation
RUN poetry config virtualenvs.create false

# Install dependencies using Poetry
RUN poetry install --no-dev --no-interaction

# Copy the rest of the application code
COPY . .

# Set environment variables for pythonpath
ENV PYTHONPATH=/app

# Run any prestart scripts
RUN poetry run bash ./prestart.sh

# Expose the application port
EXPOSE 8000

# Define the command to run the application
CMD ["poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
