# VOICE-CONTROLLER
import pyttsx3
import speech_recognition as sr
import datetime
import os
import random
from requests import get
import wikipedia
import webbrowser
import pywhatkit as kit
import smtplib
import sys
import requests
from bs4 import BeautifulSoup
import tkinter as tk

engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty("voices", voices[0].id)

# Text To Speech
def speak(audio):
    engine.say(audio)
    print(audio)
    engine.runAndWait()

# Convert Voice into Text
def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("sun rha ho ")
        r.pause_threshold = 1
        audio = r.listen(source, timeout=1, phrase_time_limit=5)
    try:
        print("Recognizer....")
        query = r.recognize_google(audio, language="en-pk")
        print("User Said: ", query)
    except Exception as e:
        speak("Say that again, please.")
        return "none"
    return query

# To Wish
def wishMe():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        speak("kia hal he, Sir")
    elif 12 <= hour < 18:
        speak("kia hal he, Sir")
    else:
        speak("kia hal he, Sir")


def start_assistant():
    speak("Hello Sir, I am apka ghulam. Please tell me, how can I help you?")

    # Run again and again
    while True:
        # Store Query
        query = takeCommand().lower()

        # Logic building For Task
        if "open notepad" in query:
            ppath = "C:\\Windows\\notepad.exe"
            os.startfile(ppath)
        elif "close notepad" in query:
            speak("Ok sir, closing Notepad")
            os.system("taskkill /f /im notepad.exe")
        elif "open command prompt" in query:
            os.system("start cmd")
        elif "close command prompt" in query:
            speak("Ok sir, closing Command prompt")
            os.system("taskkill /f /im cmd.exe")
        elif "play music" in query:
            music = "D:\\music"
            musics = os.listdir(music)
            rd = random.choice(musics)
            os.startfile(os.path.join(music, rd))
        elif "close music" in query:
            speak("Ok sir, closing Music")
            os.system("taskkill /f /im cmd.exe")
        elif "ip address" in query:
            ip = get("https://api.ipify.org").text
            speak(f"Your IP Address is {ip}")
        elif "wikipedia".lower() in query:
            speak("Search Wikipedia....")
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, "sentences=2")
            speak("According to Wikipedia")
            speak(results)
            print(results)
        elif "open youtube" in query:
            webbrowser.open("https://www.youtube.com/")
        elif "open facebook" in query:
            webbrowser.open("https://www.facebook.com/")
        elif "open my channel" in query:
            webbrowser.open("https://www.youtube.com/@irfanjunejo.")
        elif "open tiktok" in query:
            webbrowser.open("https://www.tiktok.com/")
        elif "open google" in query:
            webbrowser.open("https://www.google.com/")
            speak("Sir, what do you want to search on Google?")
            pm = takeCommand().lower()
            webbrowser.open(f"{pm}")
        elif "send message" in query:
            kit.sendwhatmsg("++92 317 1738434", "Assalam-0-Alaikum dear", 12_35)
        elif "play song on youtube" in query:
            kit.playonyt("zem tv")
        elif "time" in query:
            hour = datetime.datetime.now().strftime("%H")
            minute = datetime.datetime.now().strftime("%M")
            speak(f"Sir, the time is {hour} o'clock {minute} minutes.")
        elif "open chat GPT" in query.lower():
            speak("Opening ChatGPT...")
            webbrowser.open("https://chat.openai.com/")
        elif "temperature" in query:
            search = "temperature in Bhakkar"
            url = f"https://www.google.com/search?q={search}"
            r = requests.get(url)
            data = BeautifulSoup(r.text, "html.parser")
            temp = data.find("div", class_="BNeawe").text
            speak(f"Current {search} is {temp}")
        elif "no thanks" in query:
            speak("Thanks for using me.")
            sys.exit()
        speak("Sir, do you have any other work?")


def create_gui():
    root = tk.Tk()
    root.title("Voice Assistant")

    

    # Add Start button
    start_button = tk.Button(root, text="Start Assistant", command=start_assistant)
    start_button.pack(pady=20)

    # Add Exit button
    exit_button = tk.Button(root, text="Exit", command=root.destroy)
    exit_button.pack(pady=10)

    root.mainloop()


if __name__ == "__main__":
    wishMe()
    create_gui()
