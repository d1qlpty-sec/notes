# Python for security — libraries

Quick reference with examples. Run: `python3 script.py`.

## socket — low-level networking
Client `connect()`, server `bind→listen→accept`.
```python
from socket import *
s = socket(AF_INET, SOCK_STREAM)   # IPv4 + TCP (AF_INET6/SOCK_DGRAM = IPv6/UDP)
s.connect(("10.0.0.1", 8000)); s.send(b"hi"); data = s.recv(1024).decode(); s.close()
srv = socket(AF_INET, SOCK_STREAM)
srv.setsockopt(SOL_SOCKET, SO_REUSEADDR, 1); srv.bind(("", 8000)); srv.listen(1)
conn, addr = srv.accept()
```
Use: port scanners, reverse shells, honeypots (fake_mysql/fake_ssh in the SOC lab).

## Scapy — craft/sniff packets
```python
from scapy.all import sniff, IP, TCP, send
sniff(filter="arp", prn=lambda p: p.summary(), store=0)
send(IP(dst="10.0.0.1")/TCP(dport=80, flags="S"))
```
Use: ARP-spoof detector, custom scanners, traffic analysis. Needs root.

## requests — HTTP client
```python
import requests
r = requests.get("http://site/api", params={"id":1}, headers={"User-Agent":"x"})
r = requests.post("http://site/login", data={"user":"a","pass":"b"}, cookies={"s":"1"})
print(r.status_code, r.text, r.headers); r = requests.get(url, verify=False)
```
Use: API testing, brute/fuzzing, web automation, sessions.

## BeautifulSoup (bs4) — HTML parsing
```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(r.text, "html.parser")
soup.find_all("a"); soup.find("input", {"name":"csrf"})["value"]
```
Use: scraping, extracting tokens/links/forms.

## Selenium — browser automation
```python
from selenium import webdriver
d = webdriver.Chrome(); d.get("http://site")
d.find_element("name","username").send_keys("admin")
```
Use: JS-rendered pages, auto-login, screenshots.

## hashlib — hashing
```python
import hashlib
hashlib.sha256(b"password").hexdigest()   # md5, sha1, sha256, sha512
```
Use: compute/compare hashes, honeytokens, integrity checks.

## cryptography — encryption
```python
from cryptography.fernet import Fernet
k = Fernet.generate_key(); f = Fernet(k)
ct = f.encrypt(b"secret"); f.decrypt(ct)
```
Use: encrypt data, keys/certs, TLS.

## json — APIs (near-mandatory)
```python
import json
d = json.loads(r.text); s = json.dumps(d, indent=2)
```

## yaml — configs, DevSecOps/CI-CD
```python
import yaml
cfg = yaml.safe_load(open("config.yml"))   # safe_load, not load!
```

## csv — logs, wordlists, reports
```python
import csv
for row in csv.reader(open("data.csv")): print(row)
csv.writer(open("out.csv","w")).writerow(["ip","port"])
```

## re — regular expressions
```python
import re
re.findall(r"\b\d{1,3}(?:\.\d{1,3}){3}\b", text)   # IPs
re.search(r"password=(\w+)", text).group(1)
```
Use: parse logs, find secrets/IPs/emails, filter output.

## Practical chain
`requests` + `bs4` → grab CSRF → `requests` POST brute → `re` parse → `hashlib` verify.
Or `socket`/`scapy` for network tools.
