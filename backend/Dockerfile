FROM python:3.11.9

ENV PYTHOONUNBUFFERED 1 \
    PYTHONDONTWRITEBYTECODE 1

WORKDIR /app

RUN curl -sSL https://install.python-poetry.org | python3 -

ENV PATH="${PATH}:/root/.poetry/bin"

COPY poetry.lock pyproject.toml ./
RUN poetry install --no-dev --no-interaction

COPY . .
RUN poetry run bash ./prestart.sh

EXPOSE 8000

CMD [ "poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000" ]