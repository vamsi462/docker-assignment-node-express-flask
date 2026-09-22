#lightweight Python image
FROM python:3.8-slim-buster

# Set the working folder inside the container
WORKDIR /app

# Copy just the requirements file first
COPY requirements.txt .

# Install the packages we need
RUN pip install --no-cache-dir -r requirements.txt

# Now copy all the other files
COPY . .

# Command to run the app
ENTRYPOINT ["python", "app.py"]