FROM python:3.7-alpine as base 
WORKDIR /code
COPY requirements.txt .
RUN pip install -r requirements.txt 
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
EXPOSE 5000

FROM base as dev
ENV FLASK_DEBUG=1 
CMD ["flask", "run"]

FROM base as prd 
COPY . .
CMD ["flask", "run"]