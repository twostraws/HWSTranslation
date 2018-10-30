# Hacking with Swift Translations
### A project to create free Swift tutorials for the world

Hacking with Swift is a free online book by Paul Hudson, written in English, teaching the fundamentals of iOS app development using Swift. 

[HackingWithSwift.com](https://www.hackingwithswift.com) had 1.9 million visitors last year, spread across 6.5 million page views – all people keen to have fun learning something new, or build the app of their dreams. But in 2019 I want to take it further: I want to help people learn to build apps even if they can’t speak English.

That’s where this project comes in. This is the Hacking with Swift Translations project, where I hope to crowd-source translations of the Hacking with Swift projects in any languages where people can help. The finished translations will be released for free on the site so that everyone can enjoy learning Swift regardless of their language or financial situation.

So, I have uploaded the Markdown files for all 40 chapters in Hacking with Swift - the language introduction, plus 39 projects.

**If you want to help translate Hacking with Swift into your language, here’s what you need to do:**

1. Fork this repository.
2. Create a directory with your language code, if it doesn’t already exist – e.g. “fr” for French, “de” for German, “es” for Spanish, “it” for Italian, “zh” for Chinese, and so on.
3. Translate part or all of one Markdown file, placing your new file in your translation directory. Please keep the same filename as the English translation so we can track changes more easily, but if your translation is only partial please append “partial” to the filename – e.g. 3-5-partial.md.
4. Add a JSON file in the “contributors” directory describing you, along with your 500x500px avatar as a PNG. See paul-hudson.json for an example. Your JSON file should be named your-name.json, e.g. paul-hudson.json, and your avatar should have the same name with the extension “.png”, e.g. paul-hudson.png.
5. Open a pull request with your changes so I can merge them in.

**IMPORTANT NOTE 1:** If you open a pull request with your translation, you are granting two important rights: the right for me (Paul Hudson) to distribute your translation so that others can read it, and the right for others to modify your translation in the future. The former is important otherwise I can’t feature your work on the site, and the latter is important because as Swift continues to evolve we need to make sure the translations stay up to date.

**IMPORTANT NOTE 2:** Adding a JSON file and avatar picture is optional, but would be nice – I want to have a page on the site where I can feature everyone who helped with translations.

**IMPORTANT NOTE 3:** By contributing text here you agree that you are only translating the English, and not copying content from elsewhere. I don’t want lawsuits because someone plagiarized a different Swift tutorial.

**IMPORTANT NOTE 4:** Please do *not* modify the “en” directory – it’s there for reference only.


## What’s the goal?

Hacking with Swift has over 1200 articles at the same of writing. While all of those are free, they are all also in English, which limits who can benefit from them. 

With this project I hope we’ll be able to work together to build community translations of all 40 chapters of Hacking with Swift, so that everyone can learn Swift regardless of their language or whether they can afford to buy books. It might only be for a couple of key languages, but I hope it will still be helpful to millions of people around the world.

If things do work well, I’ll be happy to expand the project and add all the Swift Knowledge Base articles as well as Swift in Sixty Seconds, massively expanding the range of non-English material for Swift learners. And if things don’t work so well, at least we gave it a shot.

Your help is hugely appreciated, and I hope together we can deliver high-quality, free iOS tutorials to many more people around the world.


## Joining the project

To get started, all you have to do is translate part or all of the English Markdown into your native language, then open a pull request so I can merge your change back into the master repo. All files are stored as UTF-8 Markdown, which should be suitable for all world languages.

**I strongly recommend you make frequent, small commits so that there’s less chance of someone repeating your work at the same time as you.** That is, make each Markdown file one pull request rather than working on lots of them at once.

Alternatively, open a work in progress pull request using your language and chapter number, so others can see someone else is working on it.

If you’d like to play a more active role and have already made a few commits, I’d be happy to add you as a contributor to the project so you can act as a reviewer for your preferred language. If some languages end up with severl active translators, I suspect creating a Trello board or similar might work well.


## License

I want to make a few things clear up front to avoid disappointment and/or confusion later on.

- This does *not* give you the right to distribute any material here, either commercially or otherwise.
- This does however give you the ability to download all of the Hacking with Swift book for your personal offline use.
- When you commit code to this repository you are granting [Paul Hudson](https://twitter.com/twostraws) the right to distribute your translation. 
- When you add your avatar to the contributors directory you confirm you have the right to distribute that image, and are granting Paul Hudson the same right.
- When you commit code to this repository you are granting other contributors the right to modify your translation in this repository, either to make corrections, to follow Swift changes, because the original English book has changed, or other similar reasons.

**Please note:** This is new to me and I’m figuring it out as I go along, so if you have questions, suggestions, or feedback please get in touch: you can email [paul@hackingwithswift.com](mailto:paul@hackingwithswift.com) or tweet me [@twostraws](https://twitter.com/twostraws).
