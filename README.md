# Monitstatus 

**Monitstatus** is an all in one SRE observability and incident management suite.

[![Monitor detail](img/main.webp)](https://monitstatus.com/)

## Features

### Uptime monitoring

Monitor your services with periodic checks. Use it for either webpage, API, servers or network services monitoring. Define your endpoint monitors as well as the expected response and get notified when an incident ocurrs.

### Incident alerts

Be the first to know when a downtime occurs. Receive notifications of an outage via our multiple integrations: Email, SMS, Telegram, Slack.. Analyze and manage the incident cause with our incident details.

### Status pages

Beautifully crafted, customized status pages. Communicate status and incident management with your users. Build trust with every incident.

### On call schedules

*Still a work in progress.*

## Quickstart

### Repositories

Source code is split between two repositories:

 * [front](https://github.com/monitstatus/front) contains the Vue client
 * [back](https://github.com/monitstatus/back) contains the API, celery beat/workers and status pages webapp

### Dependencies

 - SendGrid api key with a validated sending domain
 - (Optional) Slack App
 - (Optional) Telegram Bot
 - (Optional) Sentry DSN

### Running from docker images

Clone the repos, manually set the envvars for both repos and launch the local environment:

    git clone git@github.com:monitstatus/back.git
    cp back/.env.sample back/.env

    # IMPORTANT!
    # edit .env file and set the required variables

    docker compose -f back/docker-compose.yml run --rm web alembic upgrade head 
    # IMPORTANT!
    # This will create a few templates on sendgrid and print a few envvars that you should copy to your .env file
    docker compose -f back/docker-compose up -d db redis
    docker compose -f back/docker-compose.yml run --rm web python app/core/generate_sendgrid_templates.py
    docker-compose -f back/docker-compose.yml up -d

    git clone git@github.com:monitstatus/front.git
    cd front
    # IMPORTANT! replace envvars during the build
    docker build \
        --build-arg API_URL=http://localhost:8000 \
        --build-arg SLACK_CLIENT_ID=$SLACK_CLIENT_SECRET \
        --build-arg STATUS_PAGES_URL=http://localhost:8001 \
        -t front .
    docker run -d -p 127.0.0.1:8080:80 --name front front

And go to http://localhost:8080
