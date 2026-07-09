
#Build stage
FROM python:3.8-slim AS build

WORKDIR /app
 
RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    libmariadb-dev \
    pkg-config

COPY . .
RUN pip install flask mysqlclient

#Production stage

FROM python:3.8-slim

WORKDIR /app
RUN apt-get update && apt-get install -y \
    libmariadb3 \
    && rm -rf /var/lib/apt/lists/*
# Copy installed Python packages
COPY --from=build /usr/local /usr/local

# Copy application
COPY --from=build /app /app

EXPOSE 5002

CMD ["python", "app.py"]
