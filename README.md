# Table of Contents

[About the Project](https://github.com/TheTimeCo-op/README/blob/master/README.md#about-the-application)

[Set Up](https://github.com/TheTimeCo-op/README/blob/master/README.md#set-up)

 - [Setting Up the Database](https://github.com/TheTimeCo-op/README/blob/master/README.md#setting-up-the-database)
 
 - [Setting Up the Application](https://github.com/TheTimeCo-op/README/blob/master/README.md#setting-up-the-application)
 
[A Walk Through](https://github.com/TheTimeCo-op/README/blob/master/README.md#a-walk-through)

 - [Server Repo](https://github.com/TheTimeCo-op/README/blob/master/README.md#server-repo)
 
 - [GUI Repo](https://github.com/TheTimeCo-op/README/blob/master/README.md#gui-repo)
 
[Creating A Component](https://github.com/TheTimeCo-op/README/blob/master/README.md#creating-a-component)

[Adding to the js_Library](https://github.com/TheTimeCo-op/README/blob/master/README.md#adding-to-the-js_Library)

[Work Flow](https://github.com/TheTimeCo-op/README/blob/master/README.md#work-flow)

[Future Ideas](https://github.com/TheTimeCo-op/README/blob/master/README.md#future-ideas)

[RULES!!!](https://github.com/TheTimeCo-op/README/blob/master/README.md#rules)

[Notes](https://github.com/TheTimeCo-op/README/blob/master/README.md#notes)

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

  From there we move into `application.ts` where we construct the `WebApi` class. Here we first import all of our npm packages and controllers before we get to the class. The class will contain a `constructor(){}` function that will be invoked as soon as the class is instantiated (essentially it's the first function called). In there we put our two private methods `this.configureMiddleware(app)` and `this.configureRoutes(app)` passing in our app instance. 

  The middleware is just our bodyparser and database connection stuff, and our routes are (surprise, surprise) our routes. We use `express.Router` because it's cleaner in my opinion. Currently there are only two routes, the time tracking functionality and the add jobs functionality. Anything that does not pertain to either of these will need it's own route i.e authentication, client stuff (if we get into that)
  
  The controllers are set up very similar as to avoid confusion (PLEASE FOLLOW THE ESTABLISHED PATTERN). We start with our imports, then go into our configuration, and then into our functions. If needed there's a section for helper functions, if you find yourself doing a certain thing twice, then it's best to pull it out into it's own function - you'll put that function in the Helper Functions section of the file. And finally at the end we have our routes, and export the router. 

  If you're unfamiliar with typescript, then I feel bad for you, because you're really missing out. Please learn more about it here: [Typescript!](https://www.typescriptlang.org/). In the `typeDefinitions` folder we have all of our interfaces, which is basically where we explain what the different object will look like. When you name an interface, it's common convention to start the name with a capital I like so: `IError` or `IJobsRawData`. These are exported, then imported (`import * as types from '../typeDefinitions/types';`) where we want them and called via `types.IJobsRawData`. PLEASE avoid using the `any` type if possible. If you everything an `any` type then that kind of defeats the purpose of using 'TYPE'script. 
   
   ## GUI Repo
   
   The GUI repo holds our actual application. It's a desktop app built with Electron. Electron uses nodejs and chromium to built the app, so people like us can come along, throw some HTML, CSS, and Javascript into a file, open it with Electron and call it a desktop app. Once you have run `npm i` which installs electron you can open it by running `npm start`. 

   In the repo you'll notice one html file and a ton of javascript files. That index file is our main html, and all the javascript does is add/remove html from it as the user clicks around. At the bottom of the file will be our script which is `bundle.js` this is a bundle file that contains all of our javascript, (it's build by running `npm run webpack` and should be built every time you want to test your change) when the project is nearing the end, we'll minify this file. So, no development should be happening here! 

   The main.js file essentially tells Electron what to do i.e. how to close, how big to make the window on start-up, where to pull the main html file from. Unless you're doing stuff that pertains to electrons interaction with our app, you don't need to mess with anything here.
   
   The renderer.js file is where we come in! This is the entry point for the app. Here we require in all of our javascript files and components. We also instantiate our component classes and do anything else that directly impacts the index.html file. 
   
   The Components folder holds all of the components. As I'm writing this there are two components `JobsCardComponent` and `JobsComponent`. In the next section I'll explain how to create a component, but it's pretty much just an ES6 class. It's best to think of the app in terms of components (or pieces that fit together). For example the `JobsCardComponent` contains the html to display the Job name and the clock in and clock out button, and the `JobsComponent` is the component that gives the `JobsCardComponent` a space to live. If we wanted to add more info to the Jobs card then we would just add to the Jobs card, or if need be we would add another component. If we wanted to add a 'Add Jobs' screen (which we do) then we would create a separte component for that. This `AddJobsComponent` we'll call it will replace the html in `<div id="main"></div>` in the `index.html` with the html in it's render function. When we want to go back to our jobs card then we'll replace the html in `<div id="main"></div>` with the `JobsComponent` html. 
   
   In the assest folder we have our style sheets and js_Library. The style sheets are pretty self-explainitory. The app uses Materialize CSS. The js_Library is where we story all of our helper functions for the GUI. Here we have eventHandlers, serverCalls, and utilities. Event handlers are similar to jQuery handlers that I wrote to make interacting with the DOM a little easier, they're pretty straight-forward and are working, so no need to mess with them, unless you're adding more! The server calls are for our GET, POST, and PUT methods. And finally utilies are for various things that will be used a lot and may be easier having a method for it. **NOTE:** If you find yourself writing something twice then it might be a good idea to turn it into a function - maybe throw it in utilies or eventHandlers depending on it's functionality.
   

# Creating a Component

  Creating a component is super easy if you follow this guide! 
  
  1. We start by defining a class, for example `class JobsComponent {}` Note it's common practice to capitalize the first letter of a class. 
  
  2. Then we have our constructor function. This will take in the objects that hold our methods, i.e. `http` for serverCalls, `util` for utilities, `$` for eventHandlers, and `moment` for moment.js - it's best to only pull in what you need. Then in the body of the function we define our variables. So the final function will look like this: 
  ```
    constructor(http, util, $, moment) {
      this.http = http;
      this.util = util;
      this.$ = $;
      this.moment = moment;
    }
  ```
  
  3. We then have the render function. This function is pretty basic, it just calls the renderHtml function passing in the html that we want to have displayed on the screen. The render function looks like this:
  ```
    render() {
      this.renderHtml(
          `<div class='row center-align'></div>
           <div class="row">
              <div id="jobs"></div>
           </div>`
      )
    }
  ```
  
  4. Next is the renderHtml function. Again, this function is super simple. It takes in an html string and adds that html to `<div id="main"></div>` - unless we're writing a subcomponent, in which case you'll add it to whatever element it needs to be added to. 
  
  5. Next are the functions needed to set the component up. For example, `JobsComponent` has a get jobs method that will grab all the jobs in the database and render the `JobsCardComponent`, passing in the jobs we got from the database. It'll then add that component to the div with `id="jobs"` that was just rendered in the render function above. Not all components will need a get jobs method (in fact this is probably the only one that does), but basically this is where you put any methods that are necessary to create the component. 
  
  6. The last method in the class is the listen method. This method will hold all of your event listeners, i.e. all of your click events or any other DOM manipulation function. 
  
  7. The rough outline of the component will look like this:
  
  ```
  class SomeComponent {
   
    constructor(somePackage){
      this.somePackage = somePackage
    }
    
    render() {
      this.renderHtml(
        //some html
      )
    }
  
    renderHtml(html){
      this.$.grabById('main').innerHTML = html //or whatever element you're rendering to
    }
    
    //here we have methods needed to set the component up
    
    listen() {
     //all of our event listeners here
    }
    
  }
  
  module.exports = SomeComponent
  ```

# Adding to the js_Library

  Basically if you find yourself writing something more than once you should probably think about putting it in a function or some exported method. The js_Library is just the place to put that method! This is just a folder that holds all of the helper methods that make developing easier. So, please add to it! 

# Work Flow

  Here is how the work flow is set up (the steps you'll take):
  
  1. After getting aquainted with the project (git clone and stuff), go to the Issues tab in github for whatever repo you want to work on. The issues are labeled on challenge level (1 easy - 5 hard) and have a shortcode to match them up with the corresponding issue in the other repo. For example if you are doing `5478 - Clock in button should clock you in` in the GUI repo then you'll find `5478 - Clock in endpoint should add time to database` in the server repo. That way you can finish the whole process if you want (you don't have to).
  
  2. After you've picked an issue, create a branch on github (via the Branch drop down button above the code files on the main page) naming it the something short and descriptive with the github issue number. For example, if `5478 - Clock in endpoint should add time to database` was the 24th issue in github, my branch would be `clock_in_#24` and it should be cut from Master.
  
  3. In your terminal/command line and run `git fetch origin` - this will tell you if any changes were made in the repo and will prompt you to `git pull` if so, but it might be safe to `git pull` anyways. 
  
  4. Next `git checkout <branch-name>` and this will put you on your new branch and connect it to your remote branch. 
  
  5. Do all of your work, and commit when you feel you have a good snapshot of your progress so far (it should be fairly often). When you make a commit PLEASE reference the issue you're working on, for example `git commit -m "Refs #24 blah blah blah"` This will put a reference to that commit in the issue in github so we can easily track changes.
  
  6. When you're ready to push run do  `git push origin <branch-name>` 
  
  7. Go to github and create a pull request. On the pull request screen verify everything looks good, your code is all there, and the number of commits you have looks good. Next assign me as a reviewer so I can review your code and put a little message in on what you did.
  
  8. I'll checkout the code and give you comments and whatnot - as more people get more familiar with the project then they can probably code review. This whole process is just a double checking process to make sure our code base stays clean and bug free.
  
  9. While it's pending code review, go back to the issue and make comment describing what you did (it only takes a couple sentences) and if you had to alter any database tables that other people will need to know. Then close the issue. 
  
  10. If there are comment that require you to make changes, then just go back to that branch, make your changes, do your commits and pushes as you would normally. Any pushes you make to that branch will go to the pull request as well and can be reviewed by the reviewer. Once the changes are approved, the branch will be merged and then that branch will be deleted. 

# Future Ideas

  For right now, we just need the app to track time for jobs and display the end time for the day. Simple stuff, but there are a ton of things we can do with it in the future. I really only have one idea currently, but here it is: It would be sweet to create a distributed block chain for each client where you can solidify your time at the end of the day into the block chain and clients can grab that chain and view the amount of time you spent on their project. It would increase the level of transparency, which some clients live for, unfortunately. Once you add the time for the day to the block chain there's no changing it, so no one can argue about the final hours (unless they claim you just sat at your computer doing nothing all day) Basically I want to try building a block chain that doesn't involve money and this could give me a reason to do it. It would be built in python and we would link it to our app via our server.

  Another idea is to host the server and database, create a login/signup/payment system and start finding users. If we decided to monetize it then each person would own a portion of the "company" based on their contributions (I really wish [Colony](https://colony.io/) was available for this reason - 2018 can't come soon enough). If we do decide to go this route then we'll have to sealup electron so you can't open developer tools, create secure login/signup systems, host the server on AWS or DigitalOcean with SSL certificates, and connect it to paypal or Stripe or something similar. We can talk about it 
  
# RULES!!!

  In order to keep things in order, we need some rules. So,
  
  1. If you push directly to master, you'll be off the project - for realsies. It's reckless and shows an inattention to detail. Also, if your change has bugs in it then those bugs will be in master, and that is not good. All additions will start on their own branch, go through a pull request and review, and will then be merged into master. 
  
  2. Only assign yourself to one github issue at a time (or one from each repo). 
  
# Notes

  * Make sure you `git fetch origin` and `git pull` often!! At least everyday and before you push. It's best to run into issues on your local machine than when trying to do a pull request. 
  
  * If you're writing something twice then turn it into a helper function and put it in the correct place for the repo! 

  * Please follow the established pattern! An organized code base is a thousand times easier to work with than one that has stuff everywhere or where things aren't formatted correctly. So please keep it organized and clear!
  
  * do 
  ```
  /**
  *
  *
  * @description
  */
  ```
  commands above every function you write to keep things clear and everyone informed.
  
  * Always ask questions if you have them! If people really like working on this then we might want to create a slack channel. 
