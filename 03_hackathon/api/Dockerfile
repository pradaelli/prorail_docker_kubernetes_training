FROM python:3.9-slim

WORKDIR /usr/src/api

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "api:api", "--host=0.0.0.0"]
