# Python для безопасности — библиотеки

Краткий справочник ключевых библиотек с примерами. Запуск: `python3 script.py`.

## socket — сетевые сокеты (низкий уровень)
Интерфейс для сетевого общения. Клиент `connect()`, сервер `bind→listen→accept`.
```python
from socket import *
# клиент
s = socket(AF_INET, SOCK_STREAM)   # IPv4 + TCP (AF_INET6/SOCK_DGRAM = IPv6/UDP)
s.connect(("10.0.0.1", 8000))
s.send(b"hello")                   # .encode() строку → байты
data = s.recv(1024).decode()       # приём, max 1024 байт
s.close()
# сервер
srv = socket(AF_INET, SOCK_STREAM)
srv.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)  # переиспользовать порт
srv.bind(("", 8000)); srv.listen(1)
conn, addr = srv.accept()
```
Применение: сканеры портов, reverse shell, honeypot (как fake_mysql/fake_ssh в SOC-лабе).

## Scapy — создание/перехват пакетов
```python
from scapy.all import sniff, ARP, IP, TCP, send
sniff(filter="arp", prn=lambda p: p.summary(), store=0)  # перехват
send(IP(dst="10.0.0.1")/TCP(dport=80, flags="S"))        # отправка SYN
```
Применение: ARP-спуф детектор, кастомные сканеры, анализ трафика. Нужен root.

## requests — HTTP-клиент
```python
import requests
r = requests.get("http://site/api", params={"id":1}, headers={"User-Agent":"x"})
r = requests.post("http://site/login", data={"user":"a","pass":"b"}, cookies={"s":"1"})
print(r.status_code, r.text, r.headers, r.cookies)
r = requests.get(url, verify=False)   # игнор TLS
```
Применение: тест API, брут/фаззинг, автоматизация веб-атак, работа с сессиями.

## BeautifulSoup (bs4) — парсинг HTML
```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(r.text, "html.parser")
soup.find_all("a")                 # все ссылки
soup.find("input", {"name":"csrf"})["value"]   # вытащить CSRF-токен
```
Применение: скрейпинг, извлечение токенов/ссылок/форм, разведка.

## Selenium — автоматизация браузера
```python
from selenium import webdriver
d = webdriver.Chrome()
d.get("http://site")
d.find_element("name","username").send_keys("admin")
```
Применение: когда нужен реальный браузер (JS-рендеринг), автологин, скриншоты.

## hashlib — хеширование
```python
import hashlib
hashlib.sha256(b"password").hexdigest()   # md5, sha1, sha256, sha512
```
Применение: вычисление/сравнение хешей, honeytoken (как weak_token в игре), проверка целостности.

## cryptography — шифрование
```python
from cryptography.fernet import Fernet
k = Fernet.generate_key(); f = Fernet(k)
ct = f.encrypt(b"secret"); f.decrypt(ct)   # симметричное AES
```
Применение: шифрование данных, работа с ключами/сертификатами, TLS.

## json — работа с API (почти обязательно)
```python
import json
d = json.loads(r.text)         # строка JSON → dict
s = json.dumps(d, indent=2)    # dict → строка
```
API почти всегда отдают JSON (как логи honeypot).

## yaml — конфиги, DevSecOps/CI-CD
```python
import yaml
cfg = yaml.safe_load(open("config.yml"))   # safe_load, не load!
```
Применение: конфиги, pipeline CI/CD, Docker/K8s манифесты.

## csv — логи, словари, отчёты
```python
import csv
for row in csv.reader(open("data.csv")): print(row)
csv.writer(open("out.csv","w")).writerow(["ip","port"])
```

## re — регулярные выражения
```python
import re
re.findall(r"\b\d{1,3}(?:\.\d{1,3}){3}\b", text)   # IP-адреса
re.search(r"password=(\w+)", text).group(1)        # извлечь значение
```
Применение: парсинг логов, поиск секретов/IP/email, фильтрация вывода.

## Связка на практике
`requests` + `bs4` → достать CSRF → `requests` POST брут → `re` парсить ответ →
`hashlib` проверить хеш. Или `socket`/`scapy` для сетевых инструментов.
