# ---- Build Stage ----
# Используем slim-образ для уменьшения размера и ускорения сборки
FROM node:18-slim AS builder

WORKDIR /app

# Копируем package.json и package-lock.json для кеширования зависимостей
COPY package*.json ./

# Устанавливаем только production-зависимости
RUN npm install --only=production

# Копируем остальной код приложения
COPY . .

# ---- Final Stage ----
# Используем тот же slim-образ для консистентности
FROM builder AS app

WORKDIR /app

# Создаем непривилегированного пользователя и группу для запуска приложения
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 appuser --ingroup nodejs

USER appuser

# Открываем порт, на котором работает приложение
EXPOSE 3000

# Команда для запуска приложения
CMD [ "npm", "start" ]