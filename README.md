#python
import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import os
import random

# Initialize text-to-speech engine
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

def speak(text):
    engine.say(text)
    engine.runAndWait()

def greet():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        speak("Good morning!")
    elif hour >= 12 and hour < 18:
        speak("Good afternoon!")
    else:
        speak("Good evening!")
    speak("I am nova. How can I assist you?")

def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")
    except Exception as e:
        print("Sorry, I couldn't understand. Can you please repeat that?")
        return None
    return query

if __name__ == "__main__":
    greet()
    while True:
        query = listen()

        if query:
            query = query.lower()

            if 'wikipedia' in query:
                speak("Searching Wikipedia...")
                query = query.replace("wikipedia", "")
                results = wikipedia.summary(query, sentences=2)
                speak("According to Wikipedia")
                print(results)
                speak(results)

            elif 'open youtube' in query:
                webbrowser.open("youtube.com")
            
            elif 'open google' in query:
                webbrowser.open("google.com")

            elif 'open calculator' in query:
                webbrowser.open ("Calculator 11.2307.4.0")

            
            elif 'play music' in query:
                music_dir = 'C:/Path/To/Music/Folder'
                songs = os.listdir(music_dir)
                random.shuffle(songs)
                os.startfile(os.path.join(music_dir, songs[0]))

            elif 'time' in query:
                time = datetime.datetime.now().strftime("%H:%M:%S")
                speak(f"The current time is {time}")

            elif 'quit' in query or 'goodbye' in query:
                speak("Goodbye!")
                break

