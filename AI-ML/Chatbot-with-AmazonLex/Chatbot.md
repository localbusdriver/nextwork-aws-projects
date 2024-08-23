# Build a Chatbot with Amazon Lex

### Banker Bot Initial Configuration
- Name: *BankerBot*
- Description: **Banker Bot to help customer check their balance and make transfers.**
- Children's Online Privacy Protection Act (COPPA): No
- Idle Session Timout: **5 minutes**

Language for Bot
- All Default
- Intent classification confidence score threshold: **0.40**

### What is intent classification confidence score threshold?
This threshold is like a minimum score for the chatbot to confidently understand what the user is trying to say.  

Setting this to 0.4 means that the chatbot need to be at least 40% confident that it understands what the user is asling to be able to give a resposne. 
Therefore if a user's input is ambiguous and the chatbot's confidene score is below 0.4, it'll throw an error message.

## Intents

### What are intents? 
An intent is what the user is trying to achieve in their conversation with the chatbot. E.g. Checking a bank account balance.   

### Configuration:
Welcome Intent
: This intent will greet the user if/when the user greets the bot.  

- Intent Name: *WelcomeIntent*
- Description: *Welcoming a user when they say hello.*
- User inputs (utterances) that will trigger this intent:
    - Hi
    - Hello
    - I need help
    - Can you help me?
- Closing Response after the intent is fulfilled:
    - Hi! I'm BB, the Banking Bot. How can I help you today?

After building the chatbot, I tried chatting with known utterances and it responded according to the trigger. 
Also, since I set the confidence threshold to 0.4, when I tried using messages similar to the known utterances, 
like "hiya", or "Help me", it would respond according to the trigger.

### Fallback Intent
Fallback intents handle responding to user inputs that the bot could not understand or inputs that resulted in a confidence score of below 40%.  

In the next step, I changed the closing response to
: Sorry I am having trouble understanding. Can you describe what you'd like to do in a few words? I can help you find your account balance, transfer funds and make a payment.

I also added variations, like: 
- Hmm could you try rephrasing that? I can help you find your account balance, transfer funds and make a payment.
- I'm sorry, I didn't get that. I can help you find your account balance, transfer funds and make a payment.  

Variations are literally variations of the same Message in the main Message box. When Amazon Lex needs to return a Fallback response, 
it will randomly choose a message from the group and return it.  

