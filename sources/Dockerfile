FROM python:stretch

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

LABEL maintainer="Phan Van Thuy ThuyPV@fpt.com"


ADD requirements.txt .
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt
COPY . /app/
WORKDIR /app

EXPOSE 80

ENTRYPOINT ["python", "app.py"]

