# voice-assistant-for-desktop code
import pyttsx3
import pyaudio
import speech_recognition as sr
import wikipedia
import datetime
import webbrowser
import random
import smtplib
import sys
import os
import pyautogui
import wmi
import psutil
import ctypes
import weather 
import re
import wolframalpha


INFO = '''
                *=======================================*
                |.....LISHA ARTIFICIAL INTELLIGENCE.....|
                +---------------------------------------+
                |        #NAME: L-I-S-H-A               |
                |        #OWNER: RAVI SINGH             |
                |        #DATE: 15/01/2019              |
                *=======================================* 
            ''' 
print(INFO)            
                     

engine = pyttsx3.init('sapi5')

client = wolframalpha.Client('V5T2P6-3VAKKLLVXU')

voices = engine.getProperty('voices')
#print(voices[1].id)
engine.setProperty('voice', voices[1].id)

def speak(audio):
    print('lisha: ' + audio)
    engine.say(audio)
    engine.runAndWait()


def wishme():
    hour = int(datetime.datetime.now().hour)
    if  hour>=0 and hour<12:
        speak("good morning ravi sir!")

    elif hour>=12 and hour<18:
        speak("good afternoon Ravi sir!")

    else:
       speak("good night ravi sir!")

wishme()

speak(" Hi i am lisha please tell me how may i help you") 

def takeCommand():
   
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)
        

    try:
        print("Recognizing.....")
        query = r.recognize_google(audio, language = 'en-in')
        print(f"user said: {query}\n")
    except Exception as e:
        #print(e)
        speak("say that again please")
        return"none"
    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('rk5892962@gmail.com', 'Mca@12306')
    server.sendmail('rk5892962@gmail.com', to, content)
    server.close()




     

if __name__ == "__main__":
     speak("ravi is good boy")
      wishme()
     while True:
     if 1:
        query = takeCommand().lower()

         Logic for executing tasks based on query
        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'open youtube' in query:
            webbrowser.open("www.youtube.com")
     
     
     
        elif 'open google' in query:
            speak('okay')
            webbrowser.open("www.google.co.in")


        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")    
            speak(f"Sir, the time is {strTime}")


        elif 'open gmail' in query:
            speak('okay')
            webbrowser.open('www.gmail.com')

        elif'open facebook' in query:
            speak('okay')
            webbrowser.open('www.facebook.com')

        elif 'open whatsapp' in query:
            speak('okay')
            webbrowser.open('www.whatsapp.com')

            
        elif "what\'s up" in query or 'how are you' in query:
            stMsgs = ['Just doing my thing!', 'I am fine!', 'Nice!', 'I am nice and full of energy']
            speak(random.choice(stMsgs))

        elif 'tell me about yourself' in query:
            speak('i am your digital assistant lisha')

        elif 'hello' in query:
            speak('Hello sir')
          
        elif 'love you' in query:
            speak('love you to sir')

        
        elif'do you have boyfriend' in query:
            speak('yes sir')

        elif'are you in relationship' in query:
            speak('no sir ,i am single')

        elif'will you marry me' in query:
            speak('no sir,ravi sir is my boyfriend')

        elif'how old are you' in query:
            speak('i am 25 years old')

        elif'who is your favourite actor' in query:
            speak('my favourite actor is salman khan')

        elif'what is your favourite food' in query:
           speak('my favourite food is litti chokha')

        elif'what is your favourite movie' in query:
           speak('my favourite movies is 3 idiots')

        elif'joke for me' in query:
            speak('ha ha ha ha ha ha ha ha lol')   


        elif'screenshot' in query:
            speak('ok,sir let me take a screenshot')
            speak('ok,done')
            speak('check your desktop, i save there ')
            pic = pyautogui.screenshot()
            pic.save('C:/Users/USER/Desktop/screenshot.png')

        elif'brightness' in query:
            if'decrease brightness' in query:
                print('ok listen...')
                dec = wmi.WMI(namespace = 'wmi')
                methods = dec.WmimonitorBrightnessMethods()[0]
                methods.WmiSetBrightness(10,0)
            
            elif'increase brightness' in query:
                print('ok listen...')
                inc = wmi.WMI(namespace = 'wmi')
                methods = inc.WmimonitorBrightnessMethods()[0]
                methods.WmiSetBrightness(100,0)
        elif'search' in query:
            webbrowser.open(query)

        elif'lock my' in query:
            speak('ok ,sir')
            ctypes.windll.user32.LockworkStation()

        elif 'current weather in' in query:
         reg_ex = re.search('current weather in (.*)', query)
         if reg_ex:
             city = reg_ex.group(1)
             weather: weather()
             location = weather.lookup_by_location(city)
             condition = location.condition()
             speak('The Current weather in %s is %s The tempeture is %.1f degree' % (city, condition.text(), (int(condition.temp())-32)/1.8))

        # elif 'play video' in query:
        #     video_folder = D:\django
        #     video = [ video 1,  video 2,  video 3]
        #     random_video = video_folder + random.choice(video) + '.mp3'
        #     os.system(random_video)


        elif 'nothing' in query or 'abort' in query or 'stop' in query:
            speak('okay')
            speak('Bye Sir, have a good day.')

        elif 'email to ravi' in query:
            try:
                speak("What should I say?")
                content = takeCommand() 
                to = "Kamit1393@gmail.com"    
                sendEmail(to, content)
                speak("Email has been sent!")
            except Exception as e:
                print(e)
                speak("Sorry my friend ravi bhai. I am not able to send this email")    

        elif 'bye' in query:
            speak('Bye Sir, have a good day.')
            sys.exit()

        elif 'thanks' in query:
            speak('welcome ravi sir')

        else:
            query = query
            speak('Searching...')
            try:
                try:
                    res = client.query(query)
                    results = next(res.results).text
                    speak('according to my database - ')
                    speak('Got it.')
                    speak(results)
                    
                except:
                    results = wikipedia.summary(query, sentences=2)
                    speak('Got it.')
                    speak('according to my database - ')
                    speak(results)
        
            except:
                webbrowser.open('www.google.com')
                

 
