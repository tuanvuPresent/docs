# Docker image Optimization

- Using alpine or slim
- Simplifying operating system dependencies
- Layer caching
- Using multistage builds

```dockerfile
FROM python:3.8-slim as builder

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc

COPY requirements.txt .
RUN pip install -r requirements.txt


FROM python:3.8-slim

COPY --from=builder /usr/local/lib/python3.8/site-packages/ /usr/local/lib/python3.8/site-packages/
COPY --from=builder /usr/local/bin/ /usr/local/bin/
WORKDIR /app
```
