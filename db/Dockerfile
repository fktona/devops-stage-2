# Use the official PostgreSQL image from Docker Hub
FROM postgres:13-alpine

# Set environment variables for PostgreSQL
ENV POSTGRES_DB=app
ENV POSTGRES_USER=app
ENV POSTGRES_PASSWORD=changethis123

# Optionally, you can add initialization scripts
# COPY init.sql /docker-entrypoint-initdb.d/

# Expose the PostgreSQL port (5432 by default)
EXPOSE 5432
