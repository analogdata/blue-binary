---
title: Creating a Middleware in FastAPI for Logging Requests and Responses
author: Rajath Kumar K S
pubDatetime: 2025-02-25T12:00:00.000+00:00
slug: fastapi-middleware-logging
featured: true
draft: false
tags:
  - Python
  - FastAPI
  - API Development
description:
  "Learn how to create a custom middleware in FastAPI to log all incoming requests and outgoing responses, including client IP and other details, into a log file."
---

<!-- # Creating a Middleware in FastAPI for Logging Requests and Responses -->

**FastAPI** is a modern, high-performance web framework for Python. One of its powerful features is the ability to customize behavior using middlewares. In this blog, we will walk through creating a middleware to log every request and response, along with useful metadata like the client's IP address, HTTP method, endpoint, and status codes.

---

## Why Use Middleware for Logging?

Middleware in FastAPI is a hook that gets executed for every request and response. This makes it an ideal place to:
- **Log Client Details**: Capture IP addresses, user agents, etc.
- **Monitor API Usage**: Track requests and responses.
- **Debugging and Auditing**: Maintain a log of all interactions with your application.

---

## Setting Up FastAPI

Before we begin, ensure you have FastAPI installed. If not, install it using:

```bash
pip install fastapi[all]
```

Let’s create a basic FastAPI app:

```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
async def read_root():
    return {"message": "Welcome to FastAPI!"}
```

<div style="text-align: center;">
  <h2>Subscribe to Python & Rest Newsletter</h2>
  <iframe src="https://embeds.beehiiv.com/c2013d8e-e443-4682-af61-a1fd014a3b15" data-test-id="beehiiv-embed" width="100%" height="320" frameborder="0" scrolling="no" style="border-radius: 4px; border: 2px solid #e5e7eb; margin: 0; background-color: transparent;"></iframe>
</div>

## Creating the Middleware

Here's how to create a middleware for logging.

### 1. **Import Required Modules**

We'll use Python's built-in logging module to log data into a file.

```python
import logging
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware
```

### 2. **Configure Logging**
Set up a log file and format the logs.

```python
logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
logger = logging.getLogger(__name__)
```

### 3. **Define the Middleware**
Create a custom middleware class that processes every request and response.

```python
class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Log request details
        client_ip = request.client.host
        method = request.method
        url = request.url.path

        logger.info(f"Request: {method} {url} from {client_ip}")
        
        # Process the request
        response = await call_next(request)
        
        # Log response details
        status_code = response.status_code
        logger.info(f"Response: {method} {url} returned {status_code} to {client_ip}")
        
        return response
```

### 4. **Add Middleware to the App**

Integrate the middleware with your FastAPI app:

```python
import logging
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware

# Configure logging
logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)
logger = logging.getLogger(__name__)

# Create FastAPI app
app = FastAPI()

# Define logging middleware
class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Log request details
        client_ip = request.client.host
        method = request.method
        url = request.url.path

        logger.info(f"Request: {method} {url} from {client_ip}")
        
        # Process the request
        response = await call_next(request)
        
        # Log response details
        status_code = response.status_code
        logger.info(f"Response: {method} {url} returned {status_code} to {client_ip}")
        
        return response

# Add middleware to the app
app.add_middleware(LoggingMiddleware)

# Define a sample route
@app.get("/")
async def read_root():
    return {"message": "Welcome to FastAPI!"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

## Testing the Middleware

### 1. Run the FastAPI App: Start the application by running the script:

```bash
uvicorn app:app --reload
```

### 2. Send Requests: Use tools like curl, Postman, or a browser to interact with the API. For example:

```bash
curl http://127.0.0.1:8000/
curl http://127.0.0.1:8000/items/42
``` 

### 3. Check the Logs: Open the app.log file to see the logged requests and responses. It will look something like this:

```text
2024-12-04 12:05:00,001 - INFO - Request: GET / from 127.0.0.1
2024-12-04 12:05:00,002 - INFO - Response: GET / returned 200 to 127.0.0.1
2024-12-04 12:06:00,003 - INFO - Request: GET /items/42 from 127.0.0.1
2024-12-04 12:06:00,004 - INFO - Response: GET /items/42 returned 200 to 127.0.0.1
```

## Conclusion

With this middleware, you can easily log all API requests and responses in your FastAPI application. This is incredibly useful for debugging, auditing, and monitoring the usage of your APIs. You can further customize the middleware to log headers, request bodies, or response payloads if needed.

Logging is an essential part of any production-grade application, and FastAPI’s middleware support makes it easy to implement.

### Follow Me on LinkedIn

<div class="badge-base LI-profile-badge" data-locale="en_US" data-size="large" data-theme="light" data-type="HORIZONTAL" data-vanity="rajathkumarks" data-version="v1"><a class="badge-base__link LI-simple-link" href="https://in.linkedin.com/in/rajathkumarks?trk=profile-badge">Rajath Kumar K S</a></div>