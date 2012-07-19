# Demo Script for Wakanda 2 Webinar: Top Ten Newest Features

Demo uses developer build 0.115372 from http://wakanda.org/downloads

Created by [Juergen Fesslmeier](mailto:jf@wakanda.org), follow me on [Twitter](http://twitter.com/chinshr)

## Creating Smartphone apps using navigation views

Adding a mobile interface to a Wakanda app is done by using the same skill sets and techniques you have learned when building desktop applications.

We will add a mobile interface to a existing application. First we will create an index page for the smart phone. We'll right click on the Web folder and select new page. You hav a choice to create smartphone or tablet, choose smartphone. You'll get a prompt for a filename, enter "index.html".

When your pages designate as mobile or tablet the widget selection changes and the properties tab of the document has mobile specific properties.

We will select Web app capable in the "iOS Metatag" Section. This allows your app to be installed as a one-click icon on iOS devices.

To add widgets use the same methods as the desktop applications. We will drag and drop a "Navigation View" widget and then a button in the lower navigation section.

Accessing the app in the browser is as simple as entering the URL http://127.0.0.1:8081/smartphone/iphone.html or drag it onto the running iOS emulator (included in Xcode on Mac OS X).

We can also install this Web application as a one click icon, because we selected them make Web app capable option (Click on bookmarks on emulator's bookmark button, and "Add to Home Screen"). Now it's easily loaded from the application launcher.

## Creating Tablet app using split-views

In the same fashion we created a Smartphone app we will add a Tablet view to our existing application.

We'll right click on the Web folder and select new page. You have a choice to create smartphone or tablet, choose tablet this time. You'll get a prompt for a filename, enter "index.html".

Drag and drop the "Split View" widget to the designer. Note that you will have iPad style vertical split view available.

Select the Company data class and drag it to the left side of the split-view, select a single attribute "Name" as the company name continue.

In the model picker expand the Company model and select related collection attribute "allEmployees" and drag it to the right side of the split-view.

Select Styles in the property editor, Size & Position, adjust the constraints to use the full size of the container area. Do the same for the Company list on the left.

Launch the app in the browser.



## Model-based permissions and restricting access

Model-based permissions allow you to restrict access to your application.

We'll create a new desktop page called "permissions.html" and drop "Company" as data-grid (remove associated attributes) and a "Login Dialog" on top of it. We will the run the page.

Our application is running in the browser with no permission applied. Even when not logged in as a user we can freely browse all available data.

In Wakanda Studio we open the directory on the Solution level and it opens in a different tab. 

In the the directory we add two new users, "Adam" and "Eve" and assign them names and passwords.

In the groups area of the directory we add an Employee group and a Manager group. In the lower-right corner include the Employee group. This means every user included in Managers group also has permissions to the Employees group. 

We'll select the Company data store class and go to the properties tab to set permissions. We'll choose the Manager group as the permission group for each of the four data operation: create, read, update, delete (CRUD)

You can also set the scope on either the data store class or an individual attribute. We'll select the attribute "revenues" and set the scope to "public on server", meaning only server-side scripts can access the attribute.

We will save and reload the model. Switch to the browser and refresh. You can already see the sale volume attribute is behaving as if it does not exist. This is because we set the scope to "public on server".

We'll login as "Adam". As you can see, Adam has no permissions to Company data.

We'll logout and login as "Eve". Eve is a manager so we're able to view companies.



## Extending data classes and restricting queries

The ability to extend data classes creates the opportunities to fine tune security, reduce code duplication, and reduce maintenance in the model.

We have two data store classes Employee and Company with several attributes.

In the model designer we'll select the "Company" data store class and created an extended class called "USCompany". Click on the gear icon and choose "Extend Company". A new data store class is created rename the class to "USCompany".

You can see that the USCompany data store class has inherited all the attributes and methods from the Company data store class. You can add new calculated attributes, and relational attributes, data store class methods to the extended class.

Now we select the USCompany data store class and add a restricting query "countryName = 'USA'"

Show the attributes showing up in the derived

Execute the following code:

    ds.USCompany.all();

Note: It shows all entities so add and add a onRestrictingQuery event handler:

    onRestrictingQuery() {
      ds.Company.query("countryName = 'USA'");
    }

Create an new interface page "restricting.html" and drop the USCompany data store to the form as grid. 

Run the page and see that only US Companies are displayed in the grid.

OR
    
Execute the following code inside a JS file to show that only US companies are returned:

    ds.USCompany.all()

## Using the Datastore Browser

The data browser is a convenient interface to review and modify data saved in your datastore. It allows you to work with your data and saves the time to build an interface yourself.

Start the Server

Open Data Browser from Studio

Browser will appear with a new tab and the data browser included.

Interface has a list with your data store classes and a tab with the initial data store class open.

You can open multiple tabs to switch content between views quickly.

When you select an entity from the list, a detail form  pops-out from the side of the list for you to edit or review the entity. You can make changes in the form or in the list directly.

When viewing an entity with a related value you can expand the auto-form to see the contents of the related entity. 

To collapse the detail form double-click on the splitter-bar.

Queries are easy to test by entering your query arguments in the top query input. We will try "name = 't*'" and click the search icon.

The data browser is also a convenient way to see how your model based permissions affect user access. Click on the login dialog to change your logged in user.

Finally, you can create a CSV export by clicking the export button. As you can see the data is exported in comma separated file (open file in editor).

http://www.wakanda.org/learning-center/essentials/using-datastore-browser



## Creating fluid layouts and aligning widgets

Constrained based layouts are layouts that use the browser window or parent containers to determine their size at runtime.

Will create a new container and go to the styles tab in the size and position area we'll click on each side to constrain the container size to that edge. We'll put a size of 10 on each side and leave the top as is, this will allow the container to size with the browser window.

We'll right click on the container and choose split vertically now we have two containers we can work with. 

We size the split (move left) and drag the company data class into the right container

We add a corresponding detail form below this grid.

For this form will constrain only the bottom edge which will make it travel in conjunction with the lower edge of the parent container.

Last we'll add a text box to the top of the smaller container, we'll constraint it to the top right which will keep it up in that corner.

When we run the application in the browser you can see our dynamic way in action. The main splitter sizes with the window. The grid and from work together and the text label remains in the top right corner of its parent container.

A great benefit of using constraint based layout is that they remain flexible with the browser's window size.

http://www.wakanda.org/learning-center/essentials/creating-fluid-layout


## Sending Email

A common feature in Web apps is to have emails automatically sent by the app. Wakanda's SMTP module allows you to send emails directly through SMTP objects.

Here I want to show you a short example of sending email using the SMTP module.

`email.js`

    var mail = require('waf-mail/mail');
    var message = new mail.Mail();
    
    message.addField('From',    'wakanda.smith@gmail.com'); 
    message.addField('To',      'jf@wakanda.org'); 
    message.addField('Subject', 'Hello from the Webinar');
    message.setBody('Hello world!');
    
    message.send('smtp.gmail.com', 465 , true, 
      "wakanda.smith@gmail.com", "anda2wak");
    message;


## Server side multi-threading using workers

Server-side workers are a very powerful and unique concept in Wakanda. It essentially provides support for multi-threading. There are several kinds of workers in Wakanda. Dedicated workers, and shared workers (and system workers, but that's quite different and would only destract us here). 

Workers in Wakanda are (almost) an identical server side implementation of the Web Workers W3 specification. The concept of dedicated workers and shared workers is similar, the main difference is in scope. Dedicated workers can only be addressed from the parent thread and are terminated when the parent thread ends. Shared workers continue to exist even if their spawning thread ends.

Worker() Example: Dedicated workers stop when the parent process ends.

`parent.js`

    console.log("starting...");
    var worker = new Worker("child.js", "child");

    worker.onmessage = function(event) {
      console.log("Message from child " + event.data);
    }

    worker.postMessage("start work");

    wait();

    console.log("finished.");

`child.js`

    onmessage = function(event) {
      console.log(event.data);
      postMessage("Hi dad!");
      close();
    }

Shared workers function across multiple threads and stay alive if you want them to. There is more info on workers in the documentation at http://doc.wakanda.org/SSJS-Modules/Workers.201-805612.en.html.

`parent.js`

    var worker = new SharedWorker("child.js", "child");

    worker.port.postMessage({to: "jfesslmeier@gmail.com", subject: "Webinar", body: "Hello Worker!"});

    debugger;

`child.js`

    onconnect = function(message) {
      var port = message.ports[0];

      port.onmessage = function(event) {
        console.log("Message received with " + event.data.to);
      }
    }



8++: Sending Email from a Shared Worker

Imaging you want to send email from you applications asynchronously. That means you will send the email without blocking the user experience.

`send.js`

    var worker = new SharedWorker("ssjs/email.js");
    var port = worker.port;

    worker.port.postMessage({to: "jf@wakanda.org", subject: "Webinar", body: "Hello World!"});

    debugger;

`email.js`

    onmessage = function(message) {
      var port = message.ports[0];
      var mail    = require('waf-mail/mail');
      var message = new mail.Mail();

      port.onmessage = function(event) {
        message.addField('From', 'wakanda.smith@gmail.com'); 
        message.addField('To', event.data.to); 
        message.addField('Subject', event.data.subject);
        message.setBody(event.data.subject);  
        message.send('smtp.gmail.com' , 465 , true, "wakanda.smith@gmail.com", "anda2wak");
        close();
      }
    }


## Nifty Editor Features

Let's see some of the editor's nifty features that could make your life a whole lot more productive!

We'll load the `ssjs/CreateData/CreateData.js` file in the editor to see larger JavaScript file inside the editor in action.

Open the code outline from the right hand side of the editor toolbar menu. Notice that the code is analyzed on the fly and automatically laid out as tree view. It gives us information about functions, variables and their data types.

Find in file and find in project. Click the gears, select "Find and Find in Files..."

Changing editor colors: Change background colors, text colors

Adjusting font-sizes: View | Bigger Fonts and Smaller Fonts

Block indentation: Select block and click tab

Block commenting: Select from the toolbar

JavaScript Code Beautifier: Open the beautify icon in the toolbar, show settings

JSLint integration: Check Errors, point out some obvious JS errors, like "!=" instead of "!==", clear the errors

Code Snippets: Make the drop-down popup the different code snippets.



## Execute JS from console

You can make Wakanda Server execute a piece of server code right from the command line.

Create a JavaScript file in an editor and add the following lines:

    console.info("Hello World!");
    console.log(process.version);

Then open a console, enter the path to the server executable, as a parameter, add the script you want to execute, such as:

    /Applications/Wakanda\ Server.app/Contents/MacOS/Wakanda\ Server ~/work/js/hello_wakanda/hello_wakanda.js 

You should see:

    Welcome to Wakanda Server!

    Hello World!

See more on this in the Documentation at http://doc.wakanda.org/Wakanda-Server-Administration/Managing-Wakanda-Server/Evaluating-a-JS-script-using-a-Command-Line.300-958090.en.html


# Credits

[Thibaud Arguillere](Thibaud.Arguillere@4d.com) thanks for the great EmployeesCompanies demo app that was used for the Webinar.

[Greg McCarvell](mailto:greg@homeslondon.com) for doing such a great job on explaining many concepts of Wakanda in his screen cast series live on the [learning center](http://www.wakanda.org/learning-center).

[David Robbins](mailto:drobbins@4d.com), for the inspirations on the material for shared workers that were derived from [Wakanda apps](http://www.wakanda.org/learning-center/best-practices) he wrote.

[Bill MacKay](mailto:bmackay@4d.com), for setting up the Webinar and being such a patient sidekick during the preparation and actual Webinar session.

