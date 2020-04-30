---
title: Creating action for Google Assistant
description: Post mortem in building action for Google Assistant
date: 2020-04-30
---

Some months ago I bought a Google Home Mini, you know the always-on-discount thing. I wanted to use it as a secretary, notifying me when I receive an email, taking my appointment, reminding me stuff. Unfortunately, [none of these features work if you have a Gsuite account and live in France](https://support.google.com/googlenest/answer/7571892?co=GENIE.Platform%3DDesktop&hl=en). As of now I mostly use it as an alarm clock, the sound is good and it's easy to put an alarm.
However, I also bought it to create some custom actions and control the most I could with my voice. After 3 months I finally decided to give it a go and built my first action. Here's my review. This is not a tutorial, the [Google documentation](https://developers.google.com/assistant/conversational/overview) is really good if you want to get started building action for your assistant.

# Context

Multiple components are working together when you say "Ok Google, put an alarm clock at 8:24 AM".
The first one will trigger to a certain sound like "Ok Google".
The second will transcribe the audio to text, going from Speech To Text (STT), also called Automatic Speech Recognition (ASR).
Then we must understand what the user said, detecting his intention like setting an alarm and at which hour. This is the Natural Language Understanding (NLU) part.
After that, we will fulfill the action, here actually setting up an alarm at 8:24 AM on our server.
Finally, we must respond to the user.

When building action for Google Assistant, Google handles the first two parts and you are responsible for the others.
You are free to use the NLU component you want. They, however, recommend Dialogflow, a company they bought in 2016, it was previously known as api.ai.
Everything is integrated with Dialogflow, if you want to use another NLU component like [RASA](https://rasa.com/) you will have to use Google's Actions SDK to register your action. When building my side project I went with Dialogflow, to ship it faster and avoid some boring configuration step.
In Dialogflow you define your intents, create an agent for a language, and for each agent and each intent you have to provide training phrases. You can also define an entity, that will be given as a parameter to your action. You don't need to provide a lot of training phrase to have something that works well, around 10 is enough. Dialogflow handles the training for you.
For each intent, you can define a simple text response or enable a webhook call.

You can host your webhook, wherever you want, I put mine on a Google Cloud Function, for the same reason I chose Dialogflow, it was integrated. It was my first step with the Google Cloud Platform.

Once you set up your server you can use two different libraries to fulfill your actions, [dialogflow](https://github.com/googleapis/nodejs-dialogflow#readme) or [actions-on-google](https://github.com/actions-on-google/actions-on-google-nodejs#readme). Using Dialogflow's library your bot could be integrated on every platform supported by Dialogflow, [here's the list](https://cloud.google.com/dialogflow/docs/integrations?hl=en). Using the actions-on-google library your bot will only work on the Google Assistant. But you can leverage every capability of it, storing data during a conversation, getting  user's data like the location, linking with a Google account or one from your organization. I used the latter because some functionalities were only available on the Assistant. Both are easy to use, you can find more recent examples with the actions-on-google library.

# My opinion

It is easy to create a simple action! The tutorial is well made and you are guided with many examples. There is a lot of advice and tips on how to build a good action, manage a conversation. Those can be used to build a great chatbot, and even to improve your real-life social skills.

The Automatic Speech Recognition worked perfectly, I always received coherent text. I was also surprised by DialogFlow intent detection, with so few training phrases, it was always able to detect the correct intention. All the hard parts worked marvelously. Unfortunately, there are some major downsides too.

First and foremost, you cannot create a private action. Your action is private during the testing phase, you can use it on a device linked to your Google account, but you cannot release it only on your device or for some handpicked accounts. If you release it, it will be public and everybody can use it. To release it, you have to provide a privacy policy, eventually a term of service, and most importantly something that can actually be used outside of your home. Alexa, the concurrent, allows private actions only if you have an [Alexa for Business](https://developer.amazon.com/en-US/docs/alexa/alexa-for-business/understand-alexa-for-business.html) account, which is expansive and only available in the US.     

Your action tends to rapidly become a complex mess if you want to do more than fetching some data on the web.
For instance, if you have to switch from one surface to another, like from a voice-only device to a screen device. You will have to handle 5 cases.

    1. Your user is already on a screen device.
    2. His voice was not recognized by Google.
    3. He has not a screen device.
    4. He did not give permission to switch.
    5. The switch happened and everything worked.

I also had to store data across conversations, there is a special object to do that. Unfortunately, it does not seem to work properly right now ([here](https://github.com/actions-on-google/actions-on-google-nodejs/issues/375) and [here](https://github.com/actions-on-google/actions-on-google-nodejs/issues/372)). To store my data, I had to setup an external database and link the user's account to get his Google ID to use it as an identifier. Before linking the user's account the application has to ask for consent, creating various possible scenarii, like described above, making the application painful to use for the user and complex for the developer.

Finally, there is a lot of latency when you trigger an action made by a third party, I experienced with most actions that I used and the one I developed.

It is really disappointing, all the hard parts are working well, but it seems the environment is not mature enough to make great actions. Leading to mostly useless or painful to use action. During the writing of this article, I discovered some tools like [Voiceflow](https://www.voiceflow.com/), [Cognigy](https://cognigy.com/) and [Jovo](https://www.jovo.tech/) maybe those are a first step towards making a better environment to build a conversational bot.

# What I built

I built a [Toggl](https://toggl.com/) bot to start and stop a timer with your voice. It took me approximately 3 days, to discover the platform and get a first version of this bot. It is going through the publication process, I will update this article when it will be out (or rejected ^^), you can check the [repository here](https://github.com/Piwit/togglgo).       
