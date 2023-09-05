FROM python:3.8.12-slim-buster
WORKDIR /app
COPY . /app
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
EXPOSE 5000
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "-c", "config.py", "app:app"]