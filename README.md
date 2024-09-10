# project-jarvis
This repository hosts a Python-based voice-activated desktop assistant, "Jarves," using pyttsx3 for speech output and speech_recognition for voice input. It performs tasks like web searches, app launches, playing music, and fetching Wikipedia summaries. Jarves also responds to personalized commands and inquiries.

import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
#print(voices[1].id)
engine.setProperty("voices", voices[1].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishme():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning")

    elif hour>=12 and hour<18:
        speak("good afternoon!")

    else:
        speak("good evening!")

    speak("Hello Rayyan Khan I am Jarves sir how may I halp you")

def takecommand():
    #it takes microphone input from user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("recognising...")
        query = r.recognize_google(audio, language='en-in')
        print(f"user said: {query}\n")

    except Exception as e:
          #print(e)
        print("say that again please... ")
        return "None"
    return query

if __name__ == "__main__":
    wishme()
    while True:
        query = takecommand().lower()

        #logic on executing task based on query

        if "wikipedia" in query:
            speak("Searching Wikipedia")
            query = query.replace("wikipedia", " ")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif "open youtube" in query:
            webbrowser.open("https://www.youtube.com/")

        elif "open today's news" in query:
            webbrowser.open("indiatoday.in")

        elif "open google" in query:
            webbrowser.open("https://www.google.com/")

        elif "play music" in query:
            webbrowser.open("https://www.youtube.com/watch?v=pT1iNnDGJbM")

        elif "play song" in query:
            webbrowser.open("https://www.youtube.com/watch?v=MA0aCUxItYA")

        elif "open whatsapp" in query:
            webbrowser.open("https://web.whatsapp.com/")

        elif "open brave" in query:
            codePath = "C:\\Program Files\\BraveSoftware\\Brave-Browser\\Application\\brave.exe"
            os.startfile(codePath)

        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")

        elif "open downloads" in query:
            codePath = "C:\\Users\\hp\\Downloads"
            os.startfile(codePath)

        elif "what's my name" in query:
            speak("Your Name is Rayyan Khan")

        elif "what's your name" in query:
            speak("My Name Is Jarves Mr Rayyan Khans Destop Assistant")

        elif "what your name" in query:
            speak("My Name Is Jarves Mr Rayyan Khans Destop Assistant")

        elif "my name" in query:
            speak("your Name is Rayyan Khan")

        elif "kaise ho" in query:
            speak("I am good how are you boss")
        
        elif "jarvis" in query:
            speak("    Yes sir")




        if "stop" in query:
             speak("Ok sir quiting")
             print("stoped")
             exit()
