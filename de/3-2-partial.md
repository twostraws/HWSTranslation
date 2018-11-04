# UIActivityViewController erklärt

Beim Teilen von Dingen in iOS wird eine standardisierte, mächtige Komponente genutzt, die andere Apps auch verwenden können. Daher sollte diese deine erste Anlaufstelle sein, wenn du das Teilen deiner App hinzufügen möchtest. Diese Komponente heißt `UIActivityViewController`: du sagst ihr, welche Art von Daten du teilen möchtest, und sie kriegt alleine raus, wie man diese am besten teilt.  

Da wir mit Bildern arbeiten, wird `UIActivityViewController` uns automatisch die Funktionalität geben, per iMessage, Email, Twitter und Facebook zu teilen, außerdem noch das Abspeichern des Bildes in der Fotobibliothek, das Zuweisen zu einem Kontakt, das Drucken via AirPrint, und mehr. Er klinkt sich sogar in AirDrop und das Erweiterungssystem von iOS ein, damit andere Apps direkt Bilder von uns einlesen können. 

Das Beste ist, es braucht nur eine Handvoll Code-Zeilen, um das alles zum Laufen zu bringen. Aber before wir `UIActivityViewController` behandeln, müssen wir zunächst den Nutzern die Möglichkeit geben, das Teilen auszulösen, und wir werden das mit einem Bar Button Item machen. 

Wenn du dich erinnerst, verwendete Projekt 1 einen `UINavigationController`, damit Nutzer zwischen zwei Bildschirmen wechseln können. Standardmäßig hat `UINavigationController` einen Streifen ("Bar") entlang der Oberseite, der `UINavigationBar` heißt, und als Entwickler können wir Knöpfe zu dieser Navigation Bar hinzufügen, um unsere Methoden aufzurufen.

Lass uns nun einen dieser Knöpfe erzeugen. Mache zunächst eine Kopie des Ordners deines bestehenden Projekt 1 (das ganze Ding), und benenne ihn in Projekt 3 um. Öffne das Projekt jetzt in Xcode, öffne die Datei DetailViewController.swift, und finde die Methode `viewDidLoad()`. Füge dies direkt unterhalb der Zeile `title = ` hinzu:

    navigationItem.rightBarButtonItem = UIBarButtonItem(barButtonSystemItem: .action, target: self, action: #selector(shareTapped))

Du wirst damit im Moment eine Fehlermeldung erhalten, aber das ist OK; bitte lese weiter.

Das kann leicht in zwei Teile aufgespalten werden: auf der linken Seite weisen wir dem `rightBarButtonItem` des `navigationItem` unseres View Controllers etwas zu. Dieses Navigationselement wird von der Navigation Bar verwendet, um relevante Informationen anzuzeigen. In diesem Fall setzen wir das rechte Bar Button Item, das ist ein Knopf, der auf der rechten Seite der Navigation Bar erscheint, wenn dieser View Controller sichtbar wird.

Auf der rechten Seite erzeugen wir eine neue Instanz des Datentyps `UIBarButtonItem` und richten sie mit drei Parametern ein: ein Systemelement ("system item"), ein Ziel ("target") und eine Aktion. Das Systemelement spezifizieren wir als `.action`, aber du kannst auch `.` eintippen und dir von der Code-Vervollständigung sagen lassen, welche weiteren Optionen verfügbar sind. Das `.action`-Systemelement zeigt einen Pfeil, der aus einem Kasten herausragt, was dem Nutzer mitteilt, dass er damit etwas machen kann, wenn er darauf tippt.

Die Parameter `target` und `action` gehen Hand in Hand, da sie in Kombination dem `UIBarButtonItem` sagen, welche Methode aufgerufen werden soll. Der `action`-Parameter sagt "Wenn du angetippt wirst, rufe die Methode `shareTapped()` auf", und der Ziel-Parameter sagt dem Knopf, dass die Methode zum momentanen View Controller gehört - `self`.

Der Teil in `#selector` erfordert mehr Erklärungen, da er aus neuer und ungewöhnlicher Syntax besteht. Er sagt dem Swift-Compiler, dass es eine Methode namens "shareTapped" geben wird, die ausgelöst werden soll, wenn der Knopf angetippt wird. Swift wird das für dich überprüfen: wenn wir aus Versehen "shareTaped" schreiben würden - ohne das zweite P - würde sich Xcode weigern, die App zu bauen, bis der Schreibfehler behoben ist.

Wenn du das Aussehen der verschiedenen verfügbaren System Bar Button Elemente nicht magst, kannst du selbst eins mit eigenem Titel und Bild erstellen. Es ist allgemein vorzuziehen, wenn möglich die Systemelemente zu benutzen, da Nutzer schon wissen, was diese machen.  

Nachdem wir den Bar-Knopf erzeugt haben, ist es jetzt Zeit, die Methode `shareTapped()` zu erzeugen. Bist du bereit für diese riesige, komplizierte Menge Code? Hier kommt er! Füge dies direkt nach der `viewWillDisappear()`-Methode ein:

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
