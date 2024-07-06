FROM python:3.11-buster

ENV PYTHOONUNBUFFERED 1 \
    PYTHONDONTWRITEBYTECODE 1

WORKDIR /usr/src/app

RUN curl -sSL https://install.python-poetry.org | python3 -

COPY poetry.lock pyproject.toml ./
RUN poetry install --without dev --no-root

COPY . .

EXPOSE 8000

RUN poetry run bash ./prestart.sh

CMD [ "poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000" ]