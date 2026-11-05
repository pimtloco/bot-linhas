import telebot
import sqlite3

# Token do seu bot do Telegram
TOKEN = "8794064819:AAFM3EHR4R0m7GgP0eZ2aLpWCD2-fjexJTg"
bot = telebot.TeleBot(TOKEN)

# Configuração do banco de dados SQLite
conn = sqlite3.connect('linhas.db', check_same_thread=False)
cursor = conn.cursor()

# Criar tabela de linhas se não existir
cursor.execute('''
    CREATE TABLE IF NOT EXISTS linhas (
        id INTEGER PRIMARY KEY,
        tipo TEXT,
        responsavel TEXT,
        status TEXT
    )
''')
conn.commit()

# Comando /start
@bot.message_handler(commands=['start'])
def send_welcome(message):
    bot.reply_to(message, "Olá! Bot de controle das 53 linhas online e operando. Use /ajuda para ver os comandos.")

# Comando /ajuda
@bot.message_handler(commands=['ajuda'])
def send_help(message):
    texto = (
        "🤖 **Comandos Disponíveis:**\n\n"
        "/consultar [número] - Consulta o status de uma linha\n"
        "/status - Lista o resumo geral"
    )
    bot.reply_to(message, texto, parse_mode="Markdown")

# Comando /consultar
@bot.message_handler(commands=['consultar'])
def consultar_linha(message):
    try:
        args = message.text.split()
        if len(args) < 2:
            bot.reply_to(message, "Por favor, informe o número da linha. Ex: /consultar 1")
            return
        
        num = args[1]
        cursor.execute("SELECT * FROM linhas WHERE id = ?", (num,))
        linha = cursor.fetchone()
        
        if linha:
            bot.reply_to(message, f"Linha {linha[0]}:\nTipo: {linha[1]}\nResponsável: {linha[2]}\nStatus: {linha[3]}")
        else:
            bot.reply_to(message, f"Linha {num} não encontrada no banco de dados.")
    except Exception as e:
        bot.reply_to(message, f"Erro ao consultar: {e}")

# Iniciar o bot
print("Bot com banco SQLite rodando...")
bot.infinity_polling()
