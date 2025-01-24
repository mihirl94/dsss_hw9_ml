This code requires ollama and tinyllama installation

from config import API_TOKEN
import telebot
import ollama

bot = telebot.TeleBot(API_TOKEN)

def ask_ollama(question):
    """
    Sends a question to the Ollama API and returns the response.
    """
    response = ollama.chat(
        model='tinyllama',  # Specify the model name
        messages=[
            {'role': 'user', 'content': question},
        ],
    )
    
    return response['message']['content']

# Handle the /start command
@bot.message_handler(commands=['start'])
def welcome(message):
    bot.send_message(message.chat.id, "Hi! I’m your AI assistant. Ask me anything!")

# Handle user messages
@bot.message_handler(func=lambda message: True)
def handle_message(message):
    user_input = message.text
    bot.send_message(message.chat.id, "Processing your request...")
    response = ask_ollama(user_input)
    bot.send_message(message.chat.id, response)

# Start polling
bot.polling()
