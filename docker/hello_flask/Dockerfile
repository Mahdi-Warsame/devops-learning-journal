FROM python:3.13
WORKDIR /app
COPY . .
RUN pip install flask mysqlclient
EXPOSE 5003
CMD ["python", "app.py"]
