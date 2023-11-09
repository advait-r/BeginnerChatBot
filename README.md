# ChatBotRepo
repository for beginner chatbot

# This is just a beginner chatbot without advanced logic or deep learning techniques. Although it uses voice commands and flexible functions it also uses text input and replies to communicate with the user.

    import sys
    import speech_recognition as sr
    from gtts import gTTS
    import playsound
    import pyaudio
    import os
    import time
    import random
    import wikipedia as wiki
    import PyQt5.QtWidgets as qtw
    import PyQt5.QtGui as qtg


    def speak(text):
        tts = gTTS(text=text)
        filename = "voice.mp3"
        tts.save(filename)
        playsound.playsound(filename)


    def get_audio():
        r = sr.Recognizer()
         with sr.Microphone() as source:
          audio = r.listen(source)
            said = ""

            try:
                said = r.recognize_google(audio)
                print(said)

            except Exception as e:
                speak("could not catch that, please say again")

        return said


    def wiki_search():
        try:
            speak("what do you want to search?")
            word = get_audio()
            info = wiki.summary(word, sentences=5)
            print(info)
            text = info
            speak("okay here you go")
            speak(text)

        except Exception as e:
            speak("could not catch that, please say again")



    class MainWindow(qtw.QWidget):

    def __init__(self):
        super().__init__()
        # title
        self.setWindowTitle("Hello User!")

        # set vertical layout (QH for horizontal)
        self.setLayout(qtw.QVBoxLayout())

        # create label
        label = qtw.QLabel("hello I am Chat")
        # change font size
        label.setFont(qtg.QFont('Helvetica', 18))
        self.layout().addWidget(label)

        #textbox
        query = qtw.QTextEdit(self,
                              lineWrapMode=qtw.QTextEdit.FixedColumnWidth,
                              lineWrapColumnOrWidth=50,
                              placeholderText="hello",
                              readOnly=False)
        self.layout().addWidget(query)

        query1 = qtw.QTextEdit(self,
                               lineWrapMode=qtw.QTextEdit.FixedColumnWidth,
                               lineWrapColumnOrWidth=50,
                               placeholderText="search/silent chat",
                               readOnly=False)
        self.layout().addWidget(query1)

        # create button
        button = qtw.QPushButton("hey chat", clicked=lambda: press_it())
        self.layout().addWidget(button)

        self.show()


        def press_it():
            label.setText(f'query: {query.toPlainText()}')
            speak("hello, how can I help you?")
            text = get_audio()

            list_responses = ["hello, how are you?", "hello I am a chat bot", "hello there! how can I help you?"]
            random_responses = random.choice(list_responses)

            if "hello" in text:
                speak(random_responses)

            if "search Wikipedia" in text:
                speak("what do you want to search?")
                label.setText("what do you want to search?")
                word = get_audio()
                info = wiki.summary(word, sentences=2)
                print(info)
                label.setText(info)
                text = info
                speak("okay here you go")
                speak(text)

            if "what is your name" in text:
                speak("my name is Chat")


        button2 = qtw.QPushButton("search wiki", clicked=lambda:press_toconfirm())
        self.layout().addWidget(button2)

        self.show()

        def press_toconfirm():

            input = query1.toPlainText()
            word = query1.toPlainText()
            info = wiki.summary(word, sentences=5)
            print(info)
            label.setText(info)
            text = info
            speak("okay here you go")
            speak(text)

        self.setFixedSize(self.layout().sizeHint())

        button3 = qtw.QPushButton("text chat", clicked=lambda:press_to_text())
        self.layout().addWidget(button3)

        self.show()

        def press_to_text():
            input_query1 = query1.toPlainText()

            list_responses = ["hello, how are you?", "hello I am a chat bot", "hello there! how can I help you?"]
            random_responses = random.choice(list_responses)

            if "hello" in input_query1:
                label.setText(random_responses)

            if "what is your name" in input_query1:
                label.setText("I am Chat")

            if "bye" in input_query1:
                label.setText("goodbye it was nice meeting you!")



        button1 = qtw.QPushButton("quit chat", clicked=lambda:press_to_quit())
        self.layout().addWidget(button1)

        self.show()

        def press_to_quit():
            speak("thank you")
            label.setText("thank you")
            sys.exit(0)


    app = qtw.QApplication([])
    mw = MainWindow()

    app.exec()

