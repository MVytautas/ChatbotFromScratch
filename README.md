# ChatbotFromScratch

https://docs.google.com/presentation/d/1-8b4KNhu5FTTwlWQ0dDni54kb-8JPnMG2v4Weu3hbgY/edit?usp=sharing

# Introduction into chatbot

You will need:
- Node.js http://nodejs.org/#download
- Visual Studio
- GitHub
- HEROKU / HEROKU CLI Link
- ngrok https://ngrok.com/download


<img src="http://s2.glbimg.com/nBsW9iMGHEMYJtADdQ9JdWXGP3k=/695x0/s.glbimg.com/po/tt2/f/original/2015/02/11/github-logo.jpg"
     alt="Markdown Monster icon"
     style="float: left; margin-right: 10px; height=42; width=42; " />
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/1200px-Node.js_logo.svg.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRl0lW8JEvQaQMh8u6VyoxEKE6BfypoCxHmpJ98_DNVcF8l0Gj6"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcScy0PJApRyEIRNdQX1Zv8AD4qoHz60-onRnPFRsc1iVxh5klC6"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />

# Websites : 

- https://www.heroku.com/
- https://developers.facebook.com/
- facebook.com

# Chatbot workflow : 
<img src="https://image.ibb.co/jyUT8U/image.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />

# Webhook workflow :
<img src="https://image.ibb.co/d1Zjhp/image.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />

# Terminal commands : 
- sudo npm install npm — global
- npm init

- OR one line ( npm install express request body-parser --save )

- npm install express 
- npm install request body-parser

# Create index.js file

```
'use strict'

const express = require('express')
const bodyParser = require('body-parser')
const request = require('request')

const app = express()

app.set('port', (process.env.PORT || 5000))

// Allows us to process the data
app.use(bodyParser.urlencoded({extended: false}))
app.use(bodyParser.json())

// ROUTES

app.get('/', function(req, res) {
	res.send("Hi I am SERMO chatbot")
})

let token = ""

// Facebook 

app.get('/webhook/', function(req, res) {
	if (req.query['hub.verify_token'] === "") {
		res.send(req.query['hub.challenge'])
	}
	res.send("Wrong token!")
})

app.post('/webhook/', function(req, res) {
	let messaging_events = req.body.entry[0].messaging
	for (let i = 0; i < messaging_events.length; i++) {
		let event = messaging_events[i]
		let sender = event.sender.id
		if (event.message && event.message.text) {
			let text = event.message.text
			sendText(sender, "Text echo: " + text.substring(0, 100))
		}
	}
	res.sendStatus(200)
})

function sendText(sender, text) {
	let messageData = {text: text}
	request({
		url: "https://graph.facebook.com/v2.6/me/messages",
		qs : {access_token: token},
		method: "POST",
		json: {
			recipient: {id: sender},
			message : messageData,
		}
	}, function(error, response, body) {
		if (error) {
			console.log("sending error")
		} else if (response.body.error) {
			console.log("response body error")
		}
	})
}

app.listen(app.get('port'), function() {
	console.log("running: port")
}) 
```
# Procfile https://drive.google.com/open?id=0Bw6T9cBdPcB8eHJEeXlPd1lMaEE

Add this line: "web: node index.js"

# Websites config:

1. Create facebook page
2. Create facebook developers app : 
<img src="https://cdn-images-1.medium.com/max/1000/1*rXG4JIZZelPOFvzXWz72KQ.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />
3. Generate token and add into index.js
<img src="https://cdn-images-1.medium.com/max/1000/1*qs5opYysQIlLYeaYqlQsaA.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />
4. Set-up webhook and add verify token into index.js
<img src="https://cdn-images-1.medium.com/max/1000/1*XGnW0OFRGBJ7NvNl2cN5vg.png"
alt="Markdown Monster icon"
style="float: left; margin-right: 10px; height=42; width=42; " />

# Trigger facebook to send messages

curl -X POST “https://graph.facebook.com/v2.6/me/subscribed_apps?access_token=<PAGE_ACCESS_TOKEN>"
