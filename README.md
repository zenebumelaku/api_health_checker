# API Health Checker

A small Express service that checks the health and response time of a configured list of APIs.

## Features

- Check all configured APIs in parallel.
- Check one API by name.
- Report HTTP status, response time, errors, and check timestamps.
- Run locally with Node.js or in Docker.

## Requirements

- Node.js 18 or later
- npm

## Getting Started

```bash
npm install
npm start
```

The server listens on `http://localhost:8080` by default. Set `PORT` to use another port:

```bash
PORT=3000 npm start
```

## API Endpoints

### Check all APIs

```http
GET /health
```

Example:

```bash
curl http://localhost:8080/health
```

The response contains `success`, the total number of configured APIs, and a `results` array. Healthy checks include the HTTP status and response time; failed checks include the error message.

### Check one API

```http
GET /health/:name
```

API names are matched case-insensitively.

```bash
curl http://localhost:8080/health/Google
```

The endpoint returns `404` when the requested API name is not configured.

## Configuration

Add or remove monitored APIs in [`apis.json`](apis.json):

```json
[
  { "name": "Google", "url": "https://www.google.com" },
  { "name": "GitHub", "url": "https://api.github.com" }
]
```

Each check uses an HTTP `GET` request with a five-second timeout. The service checks every configured API whenever an endpoint is called; it does not run on a background schedule.

## Docker

Build and run the image:

```bash
docker build -t api-health-checker .
docker run --rm -p 8080:8080 api-health-checker
```

Then call:

```bash
curl http://localhost:8080/health
```

To use a different application port, pass the same port inside and outside the container:

```bash
docker run --rm -e PORT=3000 -p 3000:3000 api-health-checker
```

## Postman

Import [`API_Health_Checker.postman_collection.json`](API_Health_Checker.postman_collection.json) into Postman to run the available health-check requests.

## Available Scripts

| Command     | Description                                      |
| ----------- | ------------------------------------------------ |
| `npm start` | Start the server                                 |
| `npm test`  | Placeholder script; tests are not configured yet |
