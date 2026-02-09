# -------- builder stage --------
FROM python:3.11-slim AS builder

WORKDIR /app

RUN apt-get update && apt-get install -y gcc

# ✅ FIX: point to correct path from build context
COPY backend/requirements.txt .

RUN pip install --user -r requirements.txt


# -------- runtime stage --------
FROM python:3.11-slim

WORKDIR /app

# create non-root user
RUN useradd -m appuser

# copy python deps
COPY --from=builder /root/.local /home/appuser/.local

# ✅ FIX: copy backend code only
COPY backend /app

# create writable db dir
RUN mkdir -p /app/data && chown -R appuser:appuser /app

ENV PATH=/home/appuser/.local/bin:$PATH
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

USER appuser

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
