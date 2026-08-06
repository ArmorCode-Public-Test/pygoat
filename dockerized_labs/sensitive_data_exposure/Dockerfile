FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Add script to make sure migrations run properly
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create a non-root user and give it ownership of the app directory
# (needed since migrations write to the sqlite db under /app at runtime)
RUN useradd --create-home --shell /bin/sh appuser \
    && chown -R appuser:appuser /app
USER appuser

EXPOSE 8000

# Use the entrypoint script
ENTRYPOINT ["/entrypoint.sh"]
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
