# ✅ ХОРОШИЙ DOCKERFILE

# --- Этап 1: builder ---
# Используем полный образ только для установки зависимостей
FROM python:3.11-slim AS builder

WORKDIR /app

# ✅ Сначала копируем ТОЛЬКО зависимости
# Этот слой кэшируется и не пересобирается если requirements.txt не менялся
COPY requirements.txt .

# Устанавливаем зависимости в отдельную папку
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# --- Этап 2: runtime ---
# Финальный образ - минимальный, только то что нужно для запуска
FROM python:3.11-slim AS runtime

WORKDIR /app

# ✅ Копируем только установленные зависимости из builder этапа
COPY --from=builder /install /usr/local

# ✅ Копируем код ПОСЛЕ зависимостей
# Изменение кода не сбрасывает кэш слоя с зависимостями
COPY app.py .

# ✅ Создаём непривилегированного пользователя
RUN useradd --no-create-home --shell /bin/false appuser
USER appuser

EXPOSE 5000

# ✅ Запускаем через gunicorn - production-ready сервер
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "2", "app:app"]
