import requests
from bs4 import BeautifulSoup
import sqlite3
from datetime import datetime
conn = sqlite3.connect('weather.db')
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS weather(date_time text, temperature real)''')
url = 'https://ua.sinoptik.ua/'
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
temperature = soup.find('span', class_='temp').get_text()
now = datetime.now()
date_time = now.strftime("%D0%BF%D0%BE%D0%B3%D0%BE%D0%B4%D0%B0-%D0%BA%D0%B8%D1%97%D0%B2")
c.execute("INSERT INTO weather (date_time, temperature) VALUES (?, ?)", (date_time, temperature))
conn.commit()
conn.close()
