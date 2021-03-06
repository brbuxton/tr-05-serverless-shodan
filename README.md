[![Travis CI Build Status](https://travis-ci.com/CiscoSecurity/tr-05-serverless-shodan.svg?branch=develop)](https://github.com/CiscoSecurity/tr-05-serverless-shodan)

# Shodan Relay API

The API itself is just a simple Flask (WSGI) application which can be easily
packaged and deployed as an AWS Lambda Function working behind an AWS API
Gateway proxy using [Zappa](https://github.com/Miserlou/Zappa).

An already deployed Relay API (e.g., packaged as an AWS Lambda Function) can
be pushed to Threat Response as a Relay Module using the
[Threat Response Relay CLI](https://github.com/threatgrid/tr-lambda-relay).

## Installation

```bash
pip install -U -r requirements.txt
```

## Testing

```bash
pip install -U -r test-requirements.txt
```

- Check for *PEP 8* compliance: `flake8 .`.
- Run the suite of unit tests: `pytest -v tests/unit/`.

## Deployment

```bash
pip install -U -r deploy-requirements.txt
```

As an AWS Lambda Function:
- Deploy: `zappa deploy dev`.
- Check: `zappa status dev`.
- Update: `zappa update dev`.
- Monitor: `zappa tail dev --http`.

As a TR Relay Module:
- Create: `relay add`.
- Update: `relay edit`.
- Delete: `relay remove`.

**Note.** For convenience, each TR Relay CLI command may be prefixed with
`env $(cat .env | xargs)` to automatically read the required environment
variables from a `.env` file (i.e.`TR_API_CLIENT_ID`, `TR_API_CLIENT_PASSWORD`,
`URL`) and pass them to the corresponding command.

## Details
The Shodan Relay API implements the following list of endpoints:
* `/refer/observables`
* `/health`.

Other endpoints (`/observe/observables`, `/deliberate/observables`, `/respond/observables`, `/respond/trigger`) 
returns empty responses.

Supported types of observables:
* `ip`,
* `domain`

Other types of observables will be ignored.

## Usage

```bash
pip install -U -r use-requirements.txt
```

```bash
export URL=<...>

http POST "${URL}"/health
http POST "${URL}"/observe/observables < observables.json
```
