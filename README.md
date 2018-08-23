# BotTalk Templates

## What is [BotTalk](https://bottalk.de)?

[BotTalk](https://bottalk.de) is the 🥇 [award-winning platform](https://www.producthunt.com/posts/bottalk)(Product Hunt Product of the Day) that allows you to create complex Alexa Skills and Google Assistant Actions with simple markup language.

Cutting-edge features:

- Session management
- Context awareness
- Third party APIs integration
- TWIG template language support
- Automated tests
- One-click deploys.

Bottalk is a FREE service.

## What are Templates?

Are you looking for a common use case for your next voice app? Or just want to learn the BotTalk syntax? We've got you covered!

We developed the Templates you can just copy and paste, adjust to your needs and launch your new Alexa Skill / Google Action in minutes!

Feel free to create your own - we're looking forward to your submissions!

## [Tip of the Day](https://github.com/bottalk/templates/tree/master/tip-of-the-day)
This template allows you to build Alexa Skills / Google Action with the daily information.
Every day of the week users can learn something new from your Skill / Action: deals, news, currency rates etc.
It's very easy to customize: every day of the week has it's own step.
Just edit the content of the ***sendText*** and/or ***sendCard*** action - and you are ready to go!

```yaml
- name: Monday Step
  actions:
    - sendText: >
          This is the placeholder for any text you want to send on Monday.
    - sendCard:
        title: 'Monday Card'
        text: >
          You can also send the cards of course
        image: "https://bottalk.de/img/bottalk_landing_logo.png"
  next: Exit
```

## [House Guest](https://github.com/bottalk/templates/tree/master/house-guest)

Are you an Airbnb host? Or are your friends or relatives just staying over? In any case - use can create and publish (!) this scenario as an Alexa Skill / Google Action to help your guests feel just like at home.

Dialogue example:

> Alexa, open House Guest

> **Alexa**: Welcome to our home! Are you looking for something? Just ask! For example:
            "Where is TV remote?" or
            "How do I lock the door?" or
            "What is the number of the host?"

> What is the WiFi password?

> **Alexa**: The WiFi password is AF340i_4v

> How do I turn on the air conditioner?

> **Alexa**:  To turn on the air conditioner just use the remote on the wall.


How to customize the scenario?

1. Edit the ```slots.yml``` file in order to add things your guests might want to find or do:

```yaml
thing_to_find:
  - 'TV remote'
  - 'WiFi password'
thing_to_do:
  - 'lock the door'
  - 'turn on the air conditioner'
```

2. Edit the ```intetns.yml``` file and come up with the variations of how your guests could ask Alexa / Google Home for instructions:

```yaml
where_is:
  - 'Where is the {thing_to_find}'
how_do_i:
  - 'How do I {thing_to_do}'
contact:
  - 'Contact'
  - 'Contact information of the host'
  - 'Call the host'
  - 'What is the number of the host'
```

3. Edit the ```main.yml``` file to provide the answers to the guests' requests. For instance, that's how "How do I..." step looks like:

```yaml
- name: How Do I Step
  actions:
    - sendText: >
        {% if thing_to_do == 'lock the door' %}
          You can lock the door with the digital key, using your mobile phone.
          Yes, we're very forward-thinking house.
        {% elseif thing_to_do == 'turn on the air conditioner' %}
          To turn on the air conditioner just use the remote on the well.
        {% endif %}
    - sendText: >
        Is there anything else I can do for you?
    - getInput:
  next:
    where_is: Where Is Step
    how_do_i: How Do I Step
    contact: Contact Step
```

## [Trivia Game](https://github.com/bottalk/templates/tree/master/trivia-game)

With this template you can build any kind of trivia game you like. In this example we play the movies trivia.

Dialogue example:

> Alexa, open Trivia Template

> **Alexa**: [Audio Effect]
            Welcome to the most insightful movies trivia game in the whole wide world!
            Do you want to start a game?

> Yes, sure

> **Alexa**: Round One! Movie Quotes!
             I give you the quote, you give me movie it came from.
             The quote: [Pause]  “You can’t handle the truth!”
             Which movie is this?
             A League of Their Own
             The Sixth Sense
             or
             A Few Good Men?

> A Few Good Men

> **Alexa**:  [Audio Effect]
            That's correct!
            Your current score is 1 point.
            Are you ready for the second round?

### How to create your own trivia game?

1. To customize the game start with editing the **First Question** step in the main.yml file:

```yaml
- name: First Question
  actions:
    - sendCard:
        title: BotTalk Trivia Template
        text: >
            Which movie is the quote "You can’t handle the truth!" from? {{ '\n\n' | raw }}

            - A League of Their Own {{ '\n' | raw }}
            - The Sixth Sense {{ '\n' | raw }}
            - A Few Good Men? {{ '\n' | raw }}
        image: 'https://bottalk.de/img/bottalk_landing_logo.png'
    - sendText: >
        Round One! Movie Quotes!
        I give you the quote, you give me movie it came from.
        The quote: <break time="1s"/>  “You can’t handle the truth!”
        Which movie is this?
        A League of Their Own
        The Sixth Sense
        or
        A Few Good Men?
    - getInput:
  next:
    first_question_answer: Check First Question
```
2. Then move on and add **first_question_answer** in the intents.yml file:

```yaml
first_question_answer:
  - '{first_question_answer_movie}'
  - 'The name of the movie was {first_question_answer_movie}'
```

3. Finish the step with editing the slots.yml and putting the **first_question_answer_movie** in - that would be your answers

```yaml
first_question_answer_movie:
  - a league of their own
  - the sixth sense
  - a few good men
```

4. To check if the answer was correct edit the **Check First Question** step in the main.yml file:

```yaml
- name: Check First Question
  actions:
    - compareContext:
        var: '{{ first_question_answer_movie | lower }}'
        is_equal: 'a few good men'
  next:
    'true': First Question Correct
    'false': First Question Incorrect
```

And just keep building your amazing game from here!