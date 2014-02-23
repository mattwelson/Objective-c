# Persistence
Persitence isn't super important for learning a language but will become useful as you start trying more ambitious projects.

Let's say you have three apps. One is a game and you want to store user settings, the second simply stores a highscore and the last one is a workout app that you want to store the user's workouts. The three options are:

1. ## Text file    
  This is probably the easiest way to start and simply works. For simple tasks like keeping track of a high score this is totally fine. Avoid this if you will be storing lots of information as reading and writing will be expensive operations. This is pretty crude, but will certainly get the job done and out the door. Your app will have its own directory where these sort of things can be stored.

  **Best for:** Protoyping and for storing small amounts of information such as in the case of a high score.  
  Information can be found here: LINK GOOD ARTICLE!  

2. ## Plist
  A plist is Apple's name for an xml file. This is probably your best option for apps that are required to store a fair amount of information, especially if it should already be on the app when the user first opens it, like an app about dunedin attractions. A plist is a list of keys with associated values, in essence a dictionary which is actually what it turns into in your program. You can read and write to a plist easily, and it can have values set when the app ships. In an app I am working on we use a plist to store the rgb values of the top and bottom colour of gradients, for example. In another we map cities to timezones.

  **Best for:** User settings and storing values that would otherwise bloat code.  
  *You could also store the high score into a plist with ease*

3. ## Core Data
  Core data is the Apple provided database/undomanager/bunchOfOtherThings. It's actually not as scary as most people think, following a tutorial like [Ray Wenderlich's](http://www.raywenderlich.com/934/core-data-tutorial-for-ios-getting-started) and taking your time will mean the learning curve really isn't so bad. If you do use it, make sure it's the right fit. This option would not be appropriate for storing a high score. It would work for storing settings but you'd have to store your default values in a plist as the database is empty when a user first opens the app.

  **Best for:** Relational stuff, storing user created events like a workout.  
  **Draw backs:** A lot harder to learn. Making changes to the database requires you to delete the old one (could be migrated, if you know how - I don't). 
