<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Jarvis</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .container {
            text-align: center;
        }
        #output {
            margin-top: 20px;
            padding: 10px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>AI Jarvis</h1>
        <button onclick="startListening()">Give Command</button>
        <div id="output"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
// Check for browser support
if (!('webkitSpeechRecognition' in window)) {
    alert("Your browser does not support the Web Speech API. Please use Chrome.");
}

const recognition = new webkitSpeechRecognition();
const speechSynthesis = window.speechSynthesis;
const output = document.getElementById('output');

recognition.continuous = false;
recognition.interimResults = false;
recognition.lang = 'en-US';

recognition.onstart = function() {
    output.textContent = 'Listening...';
};

recognition.onresult = function(event) {
    const transcript = event.results[0][0].transcript.toLowerCase();
    output.textContent = You said: ${transcript};
    handleCommand(transcript);
};

recognition.onerror = function(event) {
    output.textContent = Error occurred: ${event.error};
};

recognition.onend = function() {
    output.textContent += ' Listening ended.';
};

function startListening() {
    recognition.start();
}

function handleCommand(command) {
    let response = '';
    
    if (command.includes('hello')) {
        response = 'Hello! How can I assist you today?';
    } else if (command.includes('time')) {
        const now = new Date();
        response = The current time is ${now.toLocaleTimeString()};
    } else if (command.includes('date')) {
        const now = new Date();
        response = Today's date is ${now.toLocaleDateString()};
    } else {
        response = "I'm sorry, I didn't understand that command.";
    }
    
    speak(response);
}

function speak(text) {
    const utterance = new SpeechSynthesisUtterance(text);
    speechSynthesis.speak(utterance);
}
