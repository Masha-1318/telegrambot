import telebot 
import sympy as sp

TOKEN = "7851870785:AAG_95K_WIpHgwnrX20axNfFqCKHZKmHOdo"
bot = telebot.TeleBot(TOKEN)

@bot.message_handler(commands=['start'])
def start(message):
    bot.send_message(message.chat.id, "Привіт! Я бот. Надішліть мені математичний вираз, і я його розрахую!")

@bot.message_handler(commands=['help'])
def help_command(message):
    bot.send_message(message.chat.id, "Використовуйте команди:\n/start - почати\n/help - отримати допомогу\n/info - дізнатися більше\n/about - про бота\nАбо просто надішліть математичний приклад для розв’язання.")

@bot.message_handler(commands=['info'])
def info(message):
    bot.send_message(message.chat.id, "Цей бот допомагає з навігацією, відповідає на запити та вирішує математичні приклади, включаючи інтеграли, похідні та рівняння.")

@bot.message_handler(commands=['about'])
def about(message):
    bot.send_message(message.chat.id, "Я створений для демонстрації роботи Telegram-бота. Надішліть мені математичний вираз, і я його розрахую!")

@bot.message_handler(func=lambda message: True)
def solve_math(message):
    try:
        expression = message.text.strip()
        if "=" in expression:
            left, right = expression.split("=")
            equation = sp.Eq(sp.sympify(left), sp.sympify(right))
            solutions = sp.solve(equation)
            bot.send_message(message.chat.id, f"Розв’язок: {solutions}")
        elif "diff" in expression:
            expr = sp.sympify(expression.replace("diff", ""))
            derivative = sp.diff(expr)
            bot.send_message(message.chat.id, f"Похідна: {derivative}")
        elif "integrate" in expression:
            expr = sp.sympify(expression.replace("integrate", ""))
            integral = sp.integrate(expr)
            bot.send_message(message.chat.id, f"Інтеграл: {integral}")
        else:
            result = sp.sympify(expression).evalf()
            bot.send_message(message.chat.id, f"Результат: {result}")
    except Exception:
        bot.send_message(message.chat.id, "Помилка! Будь ласка, надішліть коректний математичний вираз.")

if __name__ == '__main__':
    bot.polling(none_stop=True)
