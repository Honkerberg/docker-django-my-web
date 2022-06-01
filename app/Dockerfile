FROM python:3.10.4-alpine as base


ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

FROM base as python-deps

RUN pip install pipenv

COPY Pipfile .
COPY Pipfile.lock .
RUN PIPENV_VENV_IN_PROJECT=1 pipenv install --deploy

FROM base as runtime

WORKDIR /usr/src/app

COPY --from=python-deps /.venv /.venv
ENV PATH="/.venv/bin:$PATH"

COPY ./entrypoint.sh .
RUN sed -i 's/\r$//g' /usr/src/app/entrypoint.sh
RUN chmod +x /usr/src/app/entrypoint.sh

RUN adduser --disabled-password --no-create-home app
USER app

COPY . .

ENTRYPOINT [ "/usr/src/app/entrypoint.sh" ]
