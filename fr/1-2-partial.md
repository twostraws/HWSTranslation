# Lister des images avec le gestionnaire de fichiers (FileManager)

Les images que je vous ai fournies proviennent de la NOAA (National Oceanic and Atmospheric Administration), une agence gouvernementale américaine qui produit du contenu dnas le domaine public que nous pouvons librement réutiliser. Une fois qu'elles ont été copiées dans votre projet, Xcode intégrera automatiquement les images dans votre application finale, lors de la phase de compilation, afin que vous puissiez y accéder.

En coulisse, une application iOS est en réalité un répertoire contenant de nombreux fichiers : le fichier binaire lui-même (c'est la version compilée de votre code, prêt à être exécutée), tout le contenu multimédia que votre application utilise, tous les fichiers relatifs à présentation visuelle, ainsi qu'une variété d'autres choses telles que des métadonnées et les droits liés à la sécurité.

Ce répertoire s'appelle un paquet (Bundle en Anglais) et porte l'extension .app. Comme nos fichiers multimédias sont perdus dans l'arborescence du répertoire, nous pouvons demander au système de nous indiquer tous les fichiers présents dans le dossier, puis d'extraire ceux que nous voulons. Vous avez peut-être remarqué que le nom de toutes les images commence par "nssl" (abréviation de National Severe Storms Laboratory). Notre tâche est donc simple : lister tous les fichiers présents dans le répertoire de notre application et extraire ceux commençant par "nssl".

Pour l'instant, nous allons charger cette liste et seulement l’afficher dans le visualiseur de logs intégré à Xcode, mais nous les ferons bientôt apparaître dans notre application.

Donc première étape : ouvrir le fichier ViewController.swift. Un contrôleur de vue (view controller) peut être considéré comme un écran contenant des informations, ce qui pour nous correspond pour le moment à notre écran tout blanc. Le fichier ViewController.swift est responsable de l’affichage de cet écran vide, et pour le moment, il ne contient pas beaucoup de code. Vous devriez voir quelque chose comme ça:

    import UIKit

    class ViewController: UIViewController {
        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view, typically from a nib.
        }
    }

That contains five interesting things I want to discuss before moving on.

1. The file starts with `import UIKit`, which means “this file will reference the iOS user interface toolkit.”
2. The `class ViewController: UIViewController` line means “I want to create a new screen of data called ViewController, based on UIViewController.” When you see a data type that starts with “UI”, it means it comes from UIKit. `UIViewController` is Apple’s default screen type, which is empty and white until we change it.
3. The line `override func viewDidLoad()` starts a method (a block of code), which is a piece of code inside our `ViewController` screen. The `override` keyword is needed because it means “we want to change Apple’s default behavior from `UIViewController`.” `viewDidLoad()` is called when the screen has loaded, and is ready for you to customize.
4. There are lots of `{` and `}` characters. These symbols, known as *braces* (or sometimes *curly brackets*) are used to mark chunks of code, and it's convention to indent lines inside braces so that it's easy to identify where code blocks start and end. The outermost braces contain the entire `ViewController` data type, and inner braces mark the start and end of the `viewDidLoad()` method.
5. The `viewDidLoad()` method contains one line of code saying `super.viewDidLoad()` and one line of comment (that’s the line starting with `//`). This `super` call mean “tell Apple’s `UIViewController` to run its own code before I run mine,” and you’ll see this used a lot.

We’ll come back to this code a *lot* in future projects; don’t worry if it’s all a bit hazy right now.

**No line numbers?** While you’re reading code, it’s frequently helpful to have line numbers enabled so you can refer to specific code more easily. If your Xcode isn't showing line numbers by default, I suggest you turn them on now: go to the Xcode menu and choose Preferences, then choose the Text Editing tab and make sure "Line numbers" is checked.

As I said before, the `viewDidLoad()` method is called when the screen has loaded and is ready for you to customize. Everything between `func viewDidLoad() {` and the `}` that follows a few lines later is part of that method, and will get called when you can start customizing the screen.

We're going to put some more code into that method to load the NSSL images. Add this beneath the line that says `super.viewDidLoad()`:

    let fm = FileManager.default
    let path = Bundle.main.resourcePath!
    let items = try! fm.contentsOfDirectory(atPath: path)

    for item in items {
        if item.hasPrefix("nssl") {
            // this is a picture to load!
        }
    }

**Note:** Some experienced Swift developers will read that code, see `try!`, then write me an angry email. If you’re considering doing just that, please continue reading first.

That’s a big chunk of code, all of which is new. Let’s walk through what it does line by line:

- The line `let fm = FileManager.default` declares a constant called `fm` and assigns it the value returned by `FileManager.default`. This is a data type that lets us work with the filesystem, and in our case we'll be using it to look for files.
- The line `let path = Bundle.main.resourcePath!` declares a constant called `path` that is set to the resource path of our app's bundle. Remember, a bundle is a directory containing our compiled program and all our assets. So, this line says, "tell me where I can find all those images I added to my app."
- The line `let items = try! fm.contentsOfDirectory(atPath: path)` declares a third constant called `items` that is set to the contents of the directory at a path. Which path? Well, the one that was returned by the line before. As you can see, Apple's long method names really does make their code quite self-descriptive! The `items` constant is an array – a collection – of the names of all the files that were found in the resource directory for our app.
- The line `for item in items {` starts a *loop*. Loops are a block of code that execute multiple times. In this case, the loop executes once for every item we found in the app bundle. Note that the line has an opening brace at the end, signaling the start of a new block of code, and there's a matching closing brace four lines beneath. Everything inside those braces will be executed each time the loop goes around. We could translate this line as "treat `items` as a series of text strings, then pull out each one of those text strings, give it the name `item`, then run the following code…"
- The line `if item.hasPrefix("nssl") {` is the first line inside our loop. By this point, we'll have the first filename ready to work with, and it'll be called `item`. To decide whether it's one we care about or not, we use the `hasPrefix()` method: it takes one parameter (the prefix to search for) and returns either true or false. That "if" at the start means this line is a conditional statement: if the item has the prefix "nssl", then… that's right, another opening brace to mark another new code block. This time, the code will be executed only if `hasPrefix()` returned true.
- Finally, the line `// this is a picture to load!` is a comment – if we reach here, `item` contains the name of a picture to load from our bundle, so we need to store it somewhere.

In just those few lines of code, there's quite a lot to take in, so before continuing let's recap:

- We use `let` to declare constants. Constants are pieces of data that we want to reference, but that we know won't have a changing value. For example, your birthday is a constant, but your age is not – your age is a variable, because it varies.
- Swift coders really like to use constants in places most other developers use variables. This is because when you're actually coding you start to realize that most of the data you store doesn't actually change very much, so you might as well make it constant. Doing so allows the system to make your code run faster, and also adds some extra safety because if you try to change a constant Xcode will refuse to build your app.
- Text in Swift is represented using the `String` data type. Swift strings are extremely powerful and guaranteed to work with any language you can think of – English, Chinese, Klingon and more.
- Collections of values are called arrays, and are usually restricted to holding one data type at a time. An array of strings is written as `[String]` and can hold only strings. If you try to put numbers in there, Xcode won’t build your app.
- The `try!` keyword means “the following code has the potential to go wrong, but I’m absolutely certain it won’t.” If the code *does* fail – for example if the directory we asked for doesn’t exist – our app will crash.
- In this case it’s perfectly fine to call `try!`, because if this code fails it means our app can't read its own data so something must be seriously wrong! Some Swift developers attempt to write code to handle these catastrophic errors at runtime, but sadly all too often they just mask the actual problem that occurred.
- You can use `for someItem in someArray` to loop through every item in an array. Swift pulls out each item and runs the code inside your loop once for each item.

If you're extremely observant you might have noticed one tiny, tiny little thing that is also one of the most complicated parts of Swift, so I'm going to keep it as simple as possible for now, then expand more over time: it's the exclamation mark at the end of `Bundle.main.resourcePath!`

No, that wasn't a typo from me. If you take away the exclamation mark the code will no longer work, so clearly Xcode thinks it's important – and indeed it is. Swift has three ways of working with data:

1.  A variable or constant that holds the data. For example, `name: String` is a string of letters called `name`.
2.  A variable or constant that might hold the data, but we're not sure. This is called an optional type, and looks like this: `name: String?` You can't use these directly, instead you need to ask Swift to check they have a value first.
3.  A variable or constant that might hold the data or might not, but we’re 100% certain it does – at least once it has first been set. This is called an implicitly unwrapped optional, and looks like this: `name: String!` You *can* use these directly.

When I explain this to people, they nearly always get confused, so please don’t worry if the above made no sense to you – we’ll be going over optionals again and again in coming projects, so just give yourself time.

We'll look at optionals in more depth later, but for now what matters is that `Bundle.main.resourcePath` may or may not return a string, so what it returns is a `String?` – that is, an optional string. By adding the exclamation mark to the end we are force unwrapping the optional string, which means we're saying, "I'm sure this will return a real string, it will never be `nil`, so please just give it to me as a regular string."

**Important warning: if you ever try to use a constant or variable that has a `nil` value, your app will crash.** As a result, some people have named `!` the "crash" operator because it's easy to get wrong. The same is true of `try!`, which is also easy to get wrong. Don't worry if this all sounds hard for now – you'll be using it more later, and it will make more sense over time.

Right now our code loads the list of files that are inside our app bundle, then loops over them all to find the ones with a name that begins with “nssl”. However, it doesn’t actually do anything with those files, so our next step is to create an array of all the “nssl” pictures so we can refer to them later rather than having to re-read the resources directory again and again.

The three constants we already created – `fm`, `path`, and `items` – live inside the `viewDidLoad()` method, and will be destroyed as soon as that method finishes. What we want is a way to attach data to the whole `ViewController` type so that it will exist for as long as our screen exists. In Swift this is done using a “property”: we can give `ViewController` as many of these properties as we want, then read and write them as often as needed while the screen exists.

To create a property, you need to declare it *outside* of methods. We’ve been creating constants using `let` so far, but this array is going to be changed inside our loop so we need to make it variable. We also need to tell Swift exactly what kind of data it will hold – in our case that’s an array of strings, where each item will be the name of an “nssl” picture.

Add this line of code *before* `viewDidLoad()`:

    var pictures = [String]()

If you’ve placed it correctly, your code should look like this:

    class ViewController: UIViewController {
        var pictures = [String]()

        override func viewDidLoad() {
            super.viewDidLoad()

            let fm = FileManager.default

The `var` keyword is used to create variables, in the same way that `let` is used to create constants. Where things get a bit crazy is in the second half of the line: `[String]()`. That’s really two things in one: `[String]` means “an array of strings”, and `()` means “create one now.” The parentheses here are just like those in the `viewDidLoad()` method – it signals the name of some other code that should be run, in this case the code to create a new array of strings.

That `pictures` array will be created when the `ViewController` screen is created, and exist for as long as the screen exists. It will be empty, because we haven’t actually filled it with anything, but at least it’s there ready for us to fill.

What we *really* want is to add to the `pictures` array all the files we match inside our loop. To do that, we need to replace the existing `// this is a picture to load!` comment with code to add each picture to the `pictures` array.

Helpfully, Swift’s arrays have a built-in method called `append` that we can use to add any items we want. So, replace the `// this is a picture to load!` comment with this:

    pictures.append(item)

That’s it! Annoyingly, after all that work our app won’t appear to do anything when you press play – you’ll see the same white screen as before. Did it work, or did things just silently fail?

To find out, add this line of code at the end of `viewDidLoad()`, just before the closing brace:

    print(pictures)

That tells Swift to print the contents of `pictures` to the Xcode debug console. When you run the program now, you should see this text appear at the bottom of your Xcode window: “["nssl0033.jpg", "nssl0034.jpg", "nssl0041.jpg", "nssl0042.jpg", "nssl0043.jpg", "nssl0045.jpg", "nssl0046.jpg", "nssl0049.jpg", "nssl0051.jpg", "nssl0091.jpg”]”

Note: iOS likes to print lots of uninteresting debug messages in the Xcode debug console. Don’t fret if you see lots of other text in there that you don’t recognize – just scroll around until you see the text above, and if you see that then you’re good to go.
