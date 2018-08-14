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

## [Tip of the Day](https://github.com/bottalk-io/templates/tree/master/tip-of-the-day)
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