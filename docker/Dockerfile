##### Django docker file template

FROM python:3.8-slim

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


COPY . /app

WORKDIR app
RUN mkdir -p ./staticfiles
RUN mkdir -p ./mediafiles

# installing dev dependencies
RUN apt-get update \
    && apt-get install -y \
        libpq-dev \
        gcc \
        libxml2-dev \
        libxslt-dev \
        libffi-dev \
        libcairo2-dev \
        libpango1.0-dev \
    && apt-get clean \
    && python3 -m venv env \
    && . ./env/bin/activate \
    && pip3 install --upgrade pip \
    && pip3 install --no-cache-dir -r requirements.txt

CMD . ./env/bin/activate \
    && python manage.py collectstatic -v 2 --noinput \
    && python manage.py migrate \
    
    # when using daphine
    # && daphne -b 0.0.0.0 -p 8000 <your-django-project>.asgi:application

    # using gunicorn
    && gunicorn <your-django-project>.wsgi:application \
        --bind 0.0.0.0:8000 \
        --workers 3 \
        --threads 3\
        --timeout 3600 \
        --log-level=debug 


# jek
