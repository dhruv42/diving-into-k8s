FROM python:3.12-alpine

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY main.py .

# Expose port 8000
EXPOSE 8000

CMD ["python", "main.py"]