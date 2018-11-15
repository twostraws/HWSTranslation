# Configurare

În acest proiect vei face o aplicație care permite utilizatorilor sa parcurgă o listă de imagini, apoi să selecteze una spre vizionare. Este intenţionat simplu, pentru că sunt multe alte lucruri pe care va trebui să le înveți pe parcurs, așa că ține-te bine - o să dureze ceva.

Lansează Xcode și alege "Create a new Xcode project" din ecranul de bun venit. Alege Single View App din listă și apasă Next. Pentru Product Name scrie Project1 și fii sigur că ai selectat Swift ca limbaj și Universal pentru dispozitive.

![Creating a new Single View App project in Xcode.](1-4.png)

One of the fields you'll be asked for is "Organization Identifier", which is a unique identifier usually made up of your personal web site domain name in reverse. For example, I would use **com.hackingwithswift** if I were making an app. You'll need to put something valid in there if you're deploying to devices, but otherwise you can just use **com.example**.

![Setting your Organization Identifier in Xcode.](1-5.png)

**Important note:** some of Xcode's project templates have checkboxes saying "Use Core Data", "Include Unit Tests" and "Include UI Tests". Please ensure these boxes are unchecked for this project and indeed almost all projects in this series – there’s only one project where this isn’t the case, and it’s made pretty clear there!

Now click Next again and you'll be asked where you want to save the project – your desktop is fine. Once that's done, you'll be presented with the example project that Xcode made for you. The first thing we need to do is make sure you have everything set up correctly, and that means running the project as-is.

When you run a project, you get to choose what kind of device the iOS Simulator should pretend to be, or you can also select a physical device if you have one plugged in. These options are listed under the Product > Destination menu, and you should see iPad Air, iPhone 8, and so on.

There's also a shortcut for this menu: at the top-left of Xcode's window is the play and stop button, but to the right of that it should say Project1 then a device name. You can click on that device name to select a different device.

**For now, please choose iPhone 8, and click the Play triangle button in the top-left corner.** This will compile your code, which is the process of converting it to instructions that iPhones can understand, then launch the simulator and run the app. As you'll see when you interact with the app, our “app” just shows a large white screen – it does nothing at all, at least not yet.

![The basic Single View App project in Xcode. Yes, it’s just a large white space.](1-6.png)

You'll be starting and stopping projects a lot as you learn, so there are three basic tips you need to know:

- You can run your project by pressing Cmd+R. This is equivalent to clicking the play button.
- You can stop a running project by pressing Cmd+. when Xcode is selected.
- If you have made changes to a running project, just press Cmd+R again. Xcode will prompt you to stop the current run before starting another. Make sure you check the "Do not show this message again" box to avoid being bothered in the future.

This project is all about letting users select images to view, so you're going to need to import some pictures. Download the files for this project from GitHub (<https://github.com/twostraws/HackingWithSwift>), and look in the “project1-files” folder. You'll see another folder in there called Content, and I’d like you to drag that Content folder straight into your Xcode project, just under where it says "Info.plist".

**Warning:** some very confused people have ignored the word “download” above and tried to drag files straight from GitHub. *That will not work*. You need to download the files as a zip file, extract them, then drag them from Finder into Xcode.

A window will appear asking how you want to add the files: make sure "Copy items if needed" is checked, and "Create groups" is selected. **Important: do not choose "Create folder references" otherwise your project will not work.**

![When you add items to Xcode, make sure you choose Create Groups.](1-7.png)

Click Finish and you'll see a yellow Content folder appear in Xcode. If you see a blue one, you didn't select "Create groups", and you'll have problems following this tutorial!

**REALLY IMPORTANT WARNING:** Several versions of Xcode managed to ship with a critical bug that affects files being added to projects, and it may already have affected you. For some reason, Xcode appears to add files to the project, but *doesn’t* add them when the project gets built, so they might as well not exist.

To find out if this has affected you, select one of the images you just added inside the project navigator, e.g. “nssl0033.jpg”. Now press Alt+Cmd+1 to activate the file inspector on the right of the Xcode window, and look for the checkbox beneath “Target Membership” – if you see an unchecked checkbox next to Project1 it means you’ve been affected by the bug.

**If this bug affects you:** fortunately the fix is really easy. After you add any files to Xcode, select them in the project navigator, go to the file inspector, then check the box under Target Membership. **You will need to keep doing this for all future files you add in all the projects in this book** – I’m sorry for all the hassle, but it’s an Xcode bug and I can’t magic it away.