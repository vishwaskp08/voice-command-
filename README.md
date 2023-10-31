# voice-command-
import webbrowser
import speech_recognition as sr
import pyttsx3
import os
import openai

# Initialize the text-to-speech engine
engine = pyttsx3.init()

# Initialize the OpenAI API client
openai.api_key = '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'

# Function to recognize speech and return the text
def get_voice_command():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
    try:
        print("Recognizing...")
        command = recognizer.recognize_google(audio)
        print("You said:", command)
        return command.lower()
    except sr.UnknownValueError:
        print("Could not understand audio. Please try again.")
        return ""

# Function to generate a response using OpenAI's GPT-3.5 API
def generate_openai_response(prompt):
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=prompt,
        max_tokens=50
    )
    return response.choices[0].text.strip()

# Function to process user input and generate a response
def generate_response(command):
    if "hello" in command:
        return "Hello! How can I assist you?"
    elif "how are you" in command:
        return "I'm just a computer program, but thanks for asking!"
    elif "what is your name" in command:
        return "I am your friendly chatbot."
    elif "exit" in command:
        return "Goodbye!"
    elif "good morning" in command:
        return "Good morning!"
    elif "open calculator" in command:
        os.system("calc")
    elif "open notepad" in command:
        os.system("notepad")
    elif "open youtube" in command:
        webbrowser.open('https://www.youtube.com/')
        
    elif "open vs code" in command:
        os.system("code") 
    elif "open whatsapp" in command:
        os.system("WhatsApp")
    elif "open browser" in command:
        os.system("start chrome")
    elif "shutdown" in command:
        os.system("shutdown /s /t 1")
    elif "restart" in command:
        os.system("shutdown /r /t 1")
    elif "log off" in command:
        os.system("shutdown /l")
    else:
        openai_prompt = f"User: {command}\nChatbot:"
        return generate_openai_response(openai_prompt)

# Main program loop
def main():
    print("Chatbot: Hello! How can I assist you?")
    while True:
        command = get_voice_command()
        if "exit" in command:
            engine.say("Goodbye!")
            engine.runAndWait()
            break
        response = generate_response(command)
        print("Chatbot:", response)
        engine.say(response)
        engine.runAndWait()

if __name__ == "__main__":
    main()
