Импорт библиотек:

import logging import requests from aiogram import Bot, Dispatcher, executor, types

logging: используется для логирования событий в приложении. requests: библиотека для выполнения HTTP-запросов. aiogram: библиотека для работы с Telegram Bot API.

Константы:

API_TOKEN = '7355985669:AAGZFCk8Rp4GmqZVH3sUJ1OZ7CTQJxKi5jo' WEATHER_API_KEY = '1f86be97bad23fc8703e21181e0d64af'

API_TOKEN: токен для аутентификации бота в Telegram. WEATHER_API_KEY: ключ API для получения данных о погоде от OpenWeatherMap.

Настройка логирования:

logging.basicConfig(level=logging.INFO)

Эта строка устанавливает уровень логирования, чтобы видеть сообщения уровня INFO и выше.

Инициализация бота и диспетчера:

bot = Bot(token=API_TOKEN) dp = Dispatcher(bot)

Создается объект Bot с токеном. Dispatcher управляет обработчиками сообщений.

Обработчик команд /start и /help:

@dp.message_handler(commands=['start', 'help']) async def send_welcome(message: types.Message): """ This handler will be called when user sends /start or /help command """ await message.reply("Hi!\nI'm EchoBot!\nPowered by aiogram.")

Этот обработчик отвечает пользователю приветственным сообщением, когда отправлены команды /start или /help.

Обработчик команды /weather:

@dp.message_handler(commands=['weather']) async def send_weather(message: types.Message): weather_info = get_weather_samara() await message.answer(weather_info)

Этот обработчик вызывает функцию get_weather_samara() и отправляет пользователю информацию о погоде.

Функция получения погоды:

def get_weather_samara(): city = "Самара" url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={WEATHER_API_KEY}&units=metric&lang=ru" response = requests.get(url)

Устанавливается переменная city с названием города. Формируется url для HTTP-запроса к API погоды.

Обработка ответа API:

if response.status_code == 200: data = response.json() temp = data['main']['temp'] feels_like = data['main']['feels_like'] weather_desc = data['weather'][0]['description'].capitalize() humidity = data['main']['humidity'] wind_speed = data['wind']['speed'] city_name = data['name'] country = data['sys']['country'] return ( f"🌤 Погода в {city_name}, {country}:\n" f"Температура: {temp}°C (ощущается как {feels_like}°C)\n" f"Описание: {weather_desc}\n" f"Влажность: {humidity}%\n" f"Скорость ветра: {wind_speed} м/с" )

Если ответ успешен (код 200), данные о погоде извлекаются из JSON-ответа. Извлекаются различные параметры погоды: температура, описание, влажность, скорость ветра и название города.

Обработка ошибки:

else: return '❌ Не удалось получить данные о погоде.'

Если произошла ошибка при запросе, возвращается сообщение об ошибке.

Запуск бота:

if name == 'main': executor.start_polling(dp, skip_updates=True) Этот блок кода запускает бота, обрабатывая входящие обновления. skip_updates=True позволяет игнорировать старые обновления, оставшиеся после оффлайн-работы.
