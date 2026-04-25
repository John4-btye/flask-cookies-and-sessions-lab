# Flask Cookies and Sessions Lab

## Overview

This project implements a backend-driven paywall system for a blog application using Flask sessions. The goal is to prevent users from bypassing frontend restrictions by enforcing article view limits at the server level.

Users are allowed to view up to three articles. After exceeding this limit, the server responds with an error message indicating that the maximum number of page views has been reached.

## Features

- Server-side session tracking using Flask's `session` object
- Article view counter stored per user session
- Paywall enforcement after 3 article views
- JSON API responses with proper HTTP status codes
- SQLite database with seeded article data

## How It Works

Each time a user requests an article:

1. The server checks if a session key (`page_views`) exists
2. If not, it initializes the counter
3. The counter increments on every request
4. Once the counter exceeds 3:
   - The server blocks access
   - Returns a `401 Unauthorized` response
   - Includes an error message

This ensures the paywall cannot be bypassed through frontend manipulation.

## Routes

### GET /articles

Returns a list of all articles.

### GET /articles/<id>

Returns a single article if the user has not exceeded the view limit.

Returns:

```json
{
  "message": "Maximum pageview limit reached"
}
```

with status code `401` if the limit is exceeded.

### GET /clear

Resets the session page view counter.

## Setup Instructions

1. Install dependencies:

   ```bash
   pipenv install && pipenv shell
   npm install --prefix client
   ```

2. Set up the database:

   ```bash
   cd server
   flask db upgrade
   python seed.py
   ```

3. Run the backend:

   ```bash
   python app.py
   ```

4. Run the frontend (in a separate terminal):

   ```bash
   npm start --prefix client
   ```

## Testing

Run the test suite with:

```bash
pytest -x
```

All tests should pass, confirming:

- Articles are returned correctly
- Session tracking increments properly
- Paywall is enforced after 3 views

## Technologies Used

- Flask
- Flask-SQLAlchemy
- Flask-Migrate
- React (frontend)
- SQLite

## Screenshots

![App Screenshot](<./client/public/screenshot.png(1).png>)

![App Screenshot](<./client/public/screenshot.png(2).png>)
