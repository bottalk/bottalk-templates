# BotTalk Templates

## What is [BotTalk](https://bottalk.de)?

[BotTalk](https://bottalk.de) is the 🥇 [award-winning platform](https://www.producthunt.com/posts/bottalk) (Product Hunt Product of the Day) that allows you to create complex Alexa Skills and Google Assistant Actions with simple markup language.

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

## [Things To Do](https://github.com/bottalk/templates/tree/master/things-to-do)

Are you at home and wondering what to do? What movie to watch? What errand to run? Or just want an idea for a healty family dinner? Then this scenario is for you!

Dialogue Example:

> Alexa, open What To Do

> **Alexa**:  Welcome to Things to Do!
              I can help you choose what to do next. Just pick a category.
              What will it be? Movies, Errands or Dinner?

> What are the top movies?

> **Alexa**:  Hm, let's see... The top movies on NetFlix this week are:
              - The Dark Knight by Christopher Nolan,
              - Star Wars: The Last Jedi by Rian Johnson,
              - Set It Up by Claire Scanlon and
              - Ex Machina by Nicholas Stoller

## How to create your own What To Do Alexa Skill / Google Home Action?

1. To customize the scenario start with editing the first `Initial step`

```yaml
  # This is the start of our scenario
  # If the step is put first - it will also be executed first
  - name: Initial step
    actions:
      # That's what Alexa will say to the user
      - sendText: >
          Welcome to Things to Do!
          I can help you choose what to do next. Just pick a category.
          What will it be? Movies, Errands or Dinner?
      # That's what we will show in Alexa Show or as Google Action Card
      # Each card has a title, text and image.
      - sendCard:
          title: Things to Do
          text: >
            Welcome to Things to Do!
            I can help you choose what to do next. Just pick a cateogory.
            What will it be?  {{ '\n' | raw }} {{ '\n' | raw }}

            - Movies {{ '\n' | raw }}
            - Errands {{ '\n' | raw }}
            - Dinner {{ '\n' | raw }}

          image: 'https://bottalk.de/img/bottalk_landing_logo.png'
      # When the user will not answer the following question correctly,
      # Alexa / Google Home could politely ask again - or reprompt
      - reprompt: >
          In order for me to suggest what you might wanna do,
          I need you to choose the category first: Movies, Errands or Dinner?
      # Here we are waiting for the user to answer our question
      - getInput:
```

2. Then Move on to the Intents secion and define a `choose_category` intent:

```yaml
choose_category:
  - '{category_name}'
```

3. Define the categories in your Slots section:

```yaml
---
slots:
  category_name:
    - 'movies'
    - 'errands'
    - 'dinner'
```

4. To make your skill more *human* introduce several intents that would *sound* like a human might say it in the free conversational manner:

```yaml
movies_intent:
  - 'what movies should i watch'
  - 'what are the top movies'
  - 'best movies'

dinner_intent:
  - 'what to cook for dinner'
  - 'what to cook today'
  - 'dinner ideas'
```

5. Finally introduce `when` - `else` [Conditional Step](https://bottalk.de/docs/#/?id=decision-making-conditional-steps) to help your skill decide which TODO item to choose from:

```yaml
  # In this step we do some conditional logic
  - name: Check If Category Is Movies
    # A user can initiate this step from ANYWHERE
    entrypoint: true
    # We first check if the user has chosen a category intent - and if so - if the category she chose is movies
    # If not - then maybe the user invoked the movies_intent directly by saying something like 'what movies should i watch'
    # To check that we use internal variable lastMessage which gives us the name of the last intent by the user
    when: 'category_name == "movies" or lastMessage == "movies_intent"'
    # WHEN the condition above is correct all of the following will be executed
    actions:
      - sendText: >
          {{ category_name }} and {{ lastMessage }}
          Hm, let's see... The top movies on NetFlix this week are:

          The Dark Knight by Christopher Nolan,
          Star Wars: The Last Jedi by Rian Johnson,
          Set It Up by Claire Scanlon and
          Ex Machina by Nicholas Stoller

          What do you want to do next?
      - sendCard:
          title: Top Movies This Week
          text: >
            {{ category_name }} and {{ lastMessage }}

            - The Dark Knight by Christopher Nolan {{ '\n' | raw }}
            - Star Wars: The Last Jedi by Rian Johnson {{ '\n' | raw }}
            - Set It Up by Claire Scanlon {{ '\n' | raw }}
            - Ex Machina by Nicholas Stoller {{ '\n' | raw }} {{ '\n' | raw }}

            Icon by http://www.designbolts.com
          image: 'https://smarthaustech.de/wp-content/uploads/2018/08/Film-icon.png'
      - reprompt: >
          In order for me to suggest what you might wanna do,
          I need you to choose the category first: Movies, Errands or Dinner?
      - getInput:
    next:
      movies_intent: Check If Category Is Movies
      errands_intent:  Check If Category Is Errands
      dinner_intent: Check If Category Is Dinner
      choose_category: Check If Category Is Movies
      # ELSE: The codition above was NOT correct - then we've got to move to the next step - where we make similar checks
      else: Check If Category Is Errands
```