FROM python:3.11-buster

ENV PYTHOONUNBUFFERED 1 \
    PYTHONDONTWRITEBYTECODE 1

WORKDIR /usr/src/app

RUN pip install poetry 

COPY poetry.lock pyproject.toml ./
RUN poetry install --without dev --no-root

ENV PYTHONPATH=/usr/src/app
COPY . .

RUN poetry run bash ./prestart.sh

CMD [ "poetry", "run", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000" ]