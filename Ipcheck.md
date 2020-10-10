# IP-Master
import requests 
import json

print("Введите айпи!")
ip = input("[IP] : ")
response = requests.get("https://ipinfo.io/" + ip + "/json")
r = response.json()
try:
  try:
    print("[ip] : " + r['ip'])
  except:
    pass
  try:   
    print("[country] : " + r['country'])
  except:
    pass
  try:  
    print("[region] : " + r['region'])
  except:
    pass
  try:   
    print("[city] : " + r['city'])
  except:
    pass
  try:   
    print("[hostname] : " + r['hostname'])
  except:
    pass
  try:   
    print("[loc] : " + r['loc'])
  except:
    pass
  try:   
    print("[org] : " + r['org'])
  except:
    pass
  try:   
    print("[timezone] : " + r['timezone'])
  except:
    pass
  try:   
    print("[postal] : " + r['postal'])
  except:
    pass
except Exeption as er:
  print(er)
