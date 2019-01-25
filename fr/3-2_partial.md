# UIActivityViewController expliqué

Le partage dans iOS utilise un composant standard puissant auquel d'autres applications peuvent se connecter. Par conséquent, il devrait s'agir de votre premier port d'escale lorsque vous ajoutez le partage à une application. Ce composant s’appelle `UIActivityViewController` : vous lui indiquez le type de données que vous souhaitez partager et il sait comment le partager au mieux.

Comme nous travaillons avec des images, `UIActivityViewController` nous donnera automatiquement les fonctionnalités pour les partager par iMessage, par e-mail, Twitter et Facebook, ou bien sauvegarder l'image dans la photothèque, l'attribuer à un contact, l'imprimer via AirPrint, et plus encore. Il est même capable de se raccrocher à AirDrop et au système d'extensions d'iOS afin que d'autres applications puissent lire l'image directement à partir de notre application.

Mieux encore, il ne faut que quelques lignes de code pour que tout fonctionne. Mais avant de toucher à `UIActivityViewController`, nous devons d’abord donner aux utilisateurs un moyen de déclencher le partage. Pour l’utiliser, nous allons ajouter un bouton à la barre de navigation (bar button item).

Si vous vous souvenez bien, le projet 1 utilisait un `UINavigationController` pour permettre aux utilisateurs de se déplacer entre deux écrans. Par défaut, un `UINavigationController` a une barre en haut, appelée `UINavigationBar`, et en tant que développeur, nous pouvons ajouter des boutons à cette barre de navigation pour qu'ils appellent nos méthodes.

Créons un de ces boutons maintenant. Commencez par prendre une copie de votre dossier Project1 existant (tout le dossier) et renommez-le en Project3. Maintenant, lancez-le dans Xcode, ouvrez le fichier DetailViewController.swift et trouvez la méthode `viewDidLoad()`. Directement sous la ligne `title =`,

    navigationItem.rightBarButtonItem = UIBarButtonItem(barButtonSystemItem: .action, target: self, action: #selector(shareTapped))

Vous aurez une erreur pour le moment, mais ça va. Lisez la suite s'il vous plaît.

This is easily split into two parts: on the left we're assigning to the `rightBarButtonItem` of our view controller's `navigationItem`. This navigation item is used by the navigation bar so that it can show relevant information. In this case, we're setting the right bar button item, which is a button that appears on the right of the navigation bar when this view controller is visible.

On the right we create a new instance of the `UIBarButtonItem` data type, setting it up with three parameters: a system item, a target, and an action. The system item we specify is `.action`, but you can type `.` to have code completion tell you the many other options available. The `.action` system item displays an arrow coming out of a box, signaling the user can do something when it's tapped.

The `target` and `action` parameters go hand in hand, because combined they tell the `UIBarButtonItem` what method should be called. The `action` parameter is saying "when you're tapped, call the `shareTapped()` method," and the target parameter tells the button that the method belongs to the current view controller – `self`.

The part in `#selector` bears explaining a bit more, because it's new and unusual syntax. What it does is tell the Swift compiler that a method called "shareTapped" will exist, and should be triggered when the button is tapped. Swift will check this for you: if we had written "shareTaped" by accident – missing the second P – Xcode will refuse to build our app until we fix the typo.

If you don't like the look of the various system bar button items available, you can create one with your own title or image instead. However, it's generally preferred to use the system items where possible because users already know what they do.

With the bar button created, it's time to create the `shareTapped()` method. Are you ready for this huge, complicated amount of code? Here goes! Put this just after the `viewWillDisappear()` method:

    @objc func shareTapped() {
        let vc = UIActivityViewController(activityItems: [imageView.image!], applicationActivities: [])
        vc.popoverPresentationController?.barButtonItem = navigationItem.rightBarButtonItem
        present(vc, animated: true)
    }

That's it. With those three lines of code, `shareTapped()` can send photos via AirDrop, post to Twitter, and much more. You have to admit, iOS can be pretty amazing sometimes!

The fourth line is old; we already learned about `present()` in project 2. However, lines 1, 2, and 3 are new, so let me explain what they do:

- Line 1 is the method name, marked with `@objc` because this method will get called by the underlying Objective-C operating system (the `UIBarButtonItem`) so we need to mark it as being available to Objective-C code. When you call a method using `#selector` you’ll always need to use `@objc` too.
- Line 2 creates a `UIActivityViewController`, which is the iOS method of sharing content with other apps and services.
- Line 3 tells iOS where the activity view controller should be anchored – where it should appear from.

On iPhone, activity view controllers automatically take up the full screen, but on iPad they appear as a popover that allows the user to see what they were working on below. This line of code tells iOS to anchor the activity view controller to the right bar button item (our share button), but this only has an effect on iPad – on iPhone it's ignored.

**Tip:** In case you were wondering, when you use `@IBAction` to make storyboards call your code, that automatically implies `@objc` – Swift knows that no `@IBAction` makes sense unless it can be called by Objective-C code.

Let's focus on how activity view controllers are created. As you can see in the code, you pass in two items: an array of items you want to share, and an array of any of your own app's services you want to make sure are in the list. We're passing an empty array into the second parameter, because our app doesn't have any services to offer. But if you were to extend this app to have something like "Other pictures like this", for example, then you would include that functionality here.

So, the real focus is on the first parameter: we're passing in `[imageView.image!]`. If you recall, the image was being displayed in a `UIImageView` called `imageView`, and `UIImageView` has an optional property called `image`, which holds a `UIImage`. But it's optional, so there may be an image or there may not. `UIActivityViewController` doesn't want “maybe or maybe not”, it wants facts – it wants a real image, not a possible image.

Fortunately, we know for a fact that our image view has an image, because we set it! In fact, that's the whole point of this view controller. So we use `imageView.image!` with that exclamation mark on the end to force unwrap the optional. That then gets put into an array by itself, and sent to `UIActivityViewController`.

And… that's it. No, really. We're pretty much done: your app now supports sharing.

Don’t worry if you’re not sure about `@objc` just yet – we’ll be coming back to it again and again. The main thing to remember is that when Swift code calls a Swift method that method doesn’t need to be marked `@objc`. On the other hand, when Objective-C code needs to call a Swift method – and that’s any time it gets called by one of Apple’s UI components, for example – then the `@objc` *is* required.

![UIActivityViewController lets your users share, print, save and more – all in just two lines of code!](3-1.png)


## Fixing a small bug

There’s one small but important bug with the current code: if you select Save Image inside the activity view controller, you’ll see the app crashes immediately. What’s happening here is that our app tries to access the user’s photo library so it can write the image there, but that isn’t allowed on iOS unless the user grants permission first.

To fix this we need to edit the Info.plist file for our project. We haven’t touched this yet, but it’s designed to hold configuration settings for your app that won’t ever change over time.

Select Info.plist in the project navigator, then when you see a big table full of data please right-click in the white space below that. Click “Add Row” from the menu that appears, and you should see a new list of options appear that starts with “Application Category”.

What I’d like you to do is scroll down in that list and find the name “Privacy - Photo Library Additions Usage Description”. This is what will be shown to the user when your app needs to add to their photo library.

When you select “Privacy - Photo Library Additions Usage Description” you’ll see “String” appear to the right of it, and to the right of “String” will be an empty white space. That white space is where you can type some text to show to the user that explains what your app intends to do with their photo library.

In this app we only need to save images, so put this text in the box: “We need to save photos you like.”

Now try running the app again, and this time selecting “Save Image” will show a message asking whether the user is OK with the app writing to their photos – much better!
