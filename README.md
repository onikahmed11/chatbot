# chatbot
// Oni Ibna Kamal, Saied Ahmmad

#lang racket

(require web-server/servlet
         web-server/servlet-env)

(define (start request)
  (response/xexpr
   `(html
     (head
      (title " Chatbot")
      (style "#chat { border:1px solid #ccc; height:200px; overflow:auto; }"))
     (body
      (h1 "Simple Chatbot")
      (div ((id "chat"))
           (p "Bot: Type anything and press Send!"))
      (input ((type "text") (id "msg")))
      (button ((onclick "reply()")) "Send")
      
      (script "
        function reply() {
          let userInput = document.getElementById('msg').value;
          let chat = document.getElementById('chat');
          
          // Add user message
          chat.innerHTML += ' ';
          
          // Simple reply logic
          let response = '';
          if (userInput.toLowerCase().includes('hi') ||   userInput.toLowerCase().includes('hello')) {
            response = 'Hello there!';
          } else if (userInput.toLowerCase().includes('bye')) {
            response = 'Goodbye!';
          } else {
            response = 'I heard: ' + userInput;
          }
          
          // Add bot response
          chat.innerHTML += ' ' + response + '';
          
          // Clear input
          document.getElementById('msg').value = '';
          
          // Scroll to bottom
          chat.scrollTop = chat.scrollHeight;
        }
        
        // Make Enter key work
        document.getElementById('msg').onkeypress = function(e) {
          if (e.keyCode == 13) reply();
        };
      ")))))

(serve/servlet start
               #:port 8080
               #:servlet-path "/"
               #:launch-browser? #t)
