FROM python:3.12

# Make directory for application
RUN mkdir /app

# Copy application files to /app
COPY ./requirements.txt /app/requirements.txt

COPY ./ /app/

# Install dependencies using pip
RUN pip install -r /app/requirements.txt

# Expose ports
EXPOSE 80

# Set the working directory of the container to /app
WORKDIR /app

# Run the server
CMD flask run -h 0.0.0.0 -p 80
