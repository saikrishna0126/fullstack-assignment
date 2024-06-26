# Use a lightweight Python image
FROM python:3.10-alpine

# Set environment variables
ENV PYTHONUNBUFFERED=1
ENV PYTHONDONTWRITEBYTECODE=1
ENV DJANGO_SECRET_KEY='kj7_@xc_gzn5@oj=am$y-((l*gpu^%c_q^=!ize)m#8376ie)%'
ENV FRONTEND_URL='http://34.174.208.0:3000'
# Set the working directory for the container
WORKDIR /app/backend

# Copy the Python dependency file into the container
COPY dependencies.txt .

# Install system dependencies required for your application
RUN apk update && \
    apk add --no-cache gcc musl-dev && \
    pip install --no-cache-dir gunicorn

# Install Python dependencies
RUN pip install --no-cache-dir -r dependencies.txt

# Copy the entire project into the container
COPY . .

# Copy the wait-for-sqlite.sh script into the container and make it executable
COPY wait-for-sqlite.sh /wait-for-sqlite.sh
RUN chmod +x /wait-for-sqlite.sh

# Copy the entrypoint.sh script into the container and make it executable
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Collect static files
RUN python manage.py collectstatic --noinput

# Expose the port that Django runs on
EXPOSE 8000

# Set the container's entrypoint to the entrypoint.sh script
ENTRYPOINT ["/entrypoint.sh"]
