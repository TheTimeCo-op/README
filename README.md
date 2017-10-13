# About the Project

  We need a name for the app! Anyway, here's the problem. I use tSheets everyday to track my time for work, and it's great, except it doesn't keep a running total of the hours each week for each job. So I find myself Friday afternoon spending half and hour adding up all the time I worked on each project. Now we have [Project Name Here]! 
  
  It's intended for freelancers just trying to track the time they spend on each project. The app will track their time, track time spent on each project for the week, month, all time. The user can enter how many hours they want to work for the week, and it'll track how much time they have left in each day and the estimated time in which they can be done each day. This will be a smart function that will adjust the end time if the user works more one day than the other. It'll track their overall working time for the day, week, month. And any other features that pop up that are cool (see future ideas at the bottom)
  
  The actual application is built on Electron (Slack, VSCode, Atom, Postman (I think) is built with it), and the server files are built with Typescript. Because the application is freestanding, I split the project into two repositories, GUI and Server. In the future we can host the server and have the GUI downloaded via Github releases. Each repository has it's own github issues to tackle and will help to keep the two parts separate. 
  
  I had a little trouble getting jQuery working with Electron (I didn't get it working; it didn't happen), so I thought it would be great to build an app all in vanilla javascript. With that said, the app uses components built with ES6 classes (similar to React) and DOM manipulation is done with "event handlers" that I wrote. As more is built, the js_Library will grow! 

Below will be how to set the app up, build in it, work with github, and future ideas I have.

# Set Up


   ## Setting Up the Database

   The app uses postgres as the database, which I personally love! It's a very reliable, elegant, sexy...And I think it's actually more popular than mySQL. 
 
   [Install Postgres on Mac](https://www.postgresql.org/download/macosx/)
   [Install Postgres on Windows](https://www.postgresql.org/download/windows/)
   [Install Postgres on Linux](https://www.postgresql.org/download/)
    
   For my postgres GUI I prefer pgAdmin. 

   [Install pgAdmin](https://www.pgadmin.org/download/)
    
   I use pgAdmin 3, but I noticed 4 is also up, so it's up to you! 
    
   Once your database is up and running, in the Server repo open `db/schema.sql` in your code editor and copy the contents into pgAdmin - hit the green play button to run the script. You might need to also insert some job names in there as well.
    
   ## Setting up the Application

   Git clone both GUI and Server repos and `npm i` both of them. 
   
   For the server repo, `npm start` (this requires `nodemon` being installed globally on your computer. I would recommend installing it if you don't already have it. If you don't want to then you'll have to rework npm start. 
   
   For the GUI repo, do `npm run webpack` to compile the code into `bundle.js` (I'll talk about this later) and then run `npm start` and electron should pop up on the screen.

# A Walk Through

   ## Server Repo
   
   The server repo holds all of our endpoints and all of our logic if we can help it (Electron is already using enough resources, so no need to push it). 
   
   We start in `start.js` where we instantiate our `WebApi` class passing in `express()` and our port. We then invoke a public method in the class `api.run()` which will start the server and listen on the port we gave it. 

  From there we move into `application.ts` where we construct the `WebApi` class. Here we first import all of our npm packages and controllers before we get to the class. The class will contain a `constructor(){}` function that will be invoked as soon as the class is instantiated
   
   ## GUI Repo
   
   

# Creating a Component

  Stuff!!!

# Adding to the js_Library

  Stuff!!!

# Work Flow

  Stuff!!!

# Future Ideas

  Stuff!!!
