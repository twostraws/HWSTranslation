# Lister des images avec le gestionnaire de fichiers (FileManager)

Les images que je vous ai fournies proviennent de la NOAA (National Oceanic and Atmospheric Administration), une agence gouvernementale américaine qui produit du contenu dans le domaine public que nous pouvons librement réutiliser. Une fois qu'elles ont été copiées dans votre projet, Xcode les intégrera automatiquement dans votre application finale, lors de la phase de compilation, afin que vous puissiez y accéder.

En coulisse, une application iOS est en réalité un répertoire contenant de nombreux fichiers : le fichier binaire lui-même (c'est la version compilée de votre code prêt à être exécuté), tout le contenu multimédia que votre application utilise, tous les fichiers relatifs à la présentation visuelle, ainsi qu'une variété d'autres choses telles que des métadonnées et les droits liés à la sécurité.

Ce répertoire s'appelle un paquet (Bundle en anglais) et porte l'extension .app. Comme nos fichiers multimédias sont perdus dans l'arborescence du répertoire, nous pouvons demander au système de nous indiquer tous les fichiers présents dans le dossier, puis d'extraire ceux que nous voulons. Vous avez peut-être remarqué que le nom de toutes les images commence par "nssl" (abréviation de National Severe Storms Laboratory). Notre tâche est donc simple : lister tous les fichiers présents dans le répertoire de notre application et extraire ceux commençant par "nssl".

Pour l'instant, nous allons charger cette liste et seulement l’afficher dans le visualiseur de logs intégré à Xcode, mais nous les ferons bientôt apparaître dans notre application.

Donc première étape : ouvrir le fichier ViewController.swift. Un contrôleur de vue (view controller) peut être considéré comme un écran contenant des informations, ce qui pour nous correspond pour le moment à notre écran tout blanc. Le fichier ViewController.swift est responsable de l’affichage de cet écran vide, et pour le moment, il ne contient pas beaucoup de code. Vous devriez voir quelque chose comme ça:

    import UIKit

    class ViewController: UIViewController {
        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view, typically from a nib.
        }
    }

Ce code contient cinq éléments intéressants dont je veux parler avant de continuer.

1. Le fichier commence par `import UIKit`, ce qui signifie que "ce fichier fera référence à la boîte à outils relative à l'interface utilisateur d'iOS."
2. La ligne `class ViewController: UIViewController` signifie "Je veux créer un nouvel écran appelé ViewController, contenant des données et basé sur UIViewController." Lorsqu'un type de données commence par "UI", cela signifie qu'il provient de UIKit. `UIViewController` est le type d'écran par défaut, vide et blanc, jusqu'à ce que nous le modifions.
3. La ligne `override func viewDidLoad()` commence une méthode (un bloc de code), qui est un morceau de code situé dans notre écran `ViewController`. Le mot-clé `override` est nécessaire car il signifie que "nous voulons modifier le comportement par défaut du `UIViewController` d'Apple". `ViewDidLoad()` est appelée lorsque l’écran a été chargé et est prêt à être personnalisé.
4. Il y a beaucoup de caractères `{` et `}`. Ces symboles, appelés *accolades*, sont utilisés pour délimiter des blocs de code. Il est généralement conseillé de mettre en retrait les lignes de code à l'intérieur des accolades afin d'identifier plus facilement où commencent et se terminent les blocs de code. Les accolades situées le plus à l'extérieur contiennent l'intégralité du code relatif au type de données `ViewController`. Les accolades situées à l'intérieur marquent le début et la fin de la méthode `viewDidLoad()`.
5. La méthode `viewDidLoad()` contient la ligne de code `super.viewDidLoad()` et un commentaire (la ligne commençant par `//`). Le terme `super` signifie "indique au `UIViewController` d'Apple d'exécuter son propre code avant que je n'exécute le mien", et vous verrez que ce mot est souvent utilisé.

Nous reviendrons souvent sur ce code dans les prochains projets ; ne vous inquiétez pas si tout est un peu flou pour le moment.

**Pas de numéro de lignes ?** Lorsque vous lisez du code, il est souvent utile d’afficher le numéro des lignes afin que vous puissiez vous référer plus facilement à un morceau de code spécifique. Si Xcode ne vous affiche pas par défaut le numéros des lignes, je vous suggère d'activer cette option dès à présent : cliquez sur Xcode dans la barre des menus et choisissez Preferences, puis cliquez sur l'onglet Text Editing et assurez-vous que "Line Numbers" est coché.

Comme je l'ai dit précédemment, la méthode `viewDidLoad()` est appelée lorsque l'écran a été chargé et est prêt à être personnalisé. Tout ce qui se trouve entre `func viewDidLoad() {` et `}` qui suit quelques lignes plus bas fait partie de cette méthode et sera appelé lorsque vous pourrez commencer à personnaliser l'écran.

Nous allons ajouter un peu de code dans cette méthode pour charger les images NSSL. Ajoutez ceci sous la ligne `super.viewDidLoad()` :

    let fm = FileManager.default
    let path = Bundle.main.resourcePath!
    let items = try! fm.contentsOfDirectory(atPath: path)

    for item in items {
        if item.hasPrefix("nssl") {
            // this is a picture to load!
        }
    }

**Remarque :** Certains développeurs en Swift expérimentés liront ce code, verront `try!`, puis m'enverront un courrier électronique incendiaire. Si vous envisagez de faire cela, lisez tout d'abord ce qui suit.

C’est un gros morceau de code, qui est complètement nouveau. Passons en revue ce qu’il fait ligne par ligne :

- La ligne `let fm = FileManager.default` déclare une constante appelée `fm` et lui assigne la valeur retournée par `FileManager.default`. C'est un type de données qui nous permet de travailler avec le système de fichiers, que nous allons utiliser dans notre cas pour rechercher des fichiers.
- La ligne `let path = Bundle.main.resourcePath!` déclare une constante appelée `path` à qui on assigne le chemin du paquet de notre application. Souvenez vous qu'un paquet est un répertoire contenant notre programme compilé et toutes nos ressources. Ainsi, cette ligne signifie : "dis-moi où je peux trouver toutes les images que j'ai ajoutées à mon application".
- La ligne `let items = try! fm.contentsOfDirectory(atPath: path)` déclare une troisième constante appelée `items` et lui assigne le contenu du répertoire situé dans path. Path correspond au chemin du paquet de notre application, qui a été retourné par la ligne précédente. Comme vous pouvez le constater, les noms des méthodes d'Apple sont relativement longs, rendant le code plus explicite ! La constante `items` est une Array - une collection - contenant les noms de tous les fichiers trouvés dans le répertoire contenant les ressources de notre application.
- La ligne `for item in items {` commence une *boucle*. Les boucles sont un bloc de code qui s'exécute plusieurs fois. Dans le cas présent, la boucle s'exécute une fois pour chaque élément trouvé dans le paquet de l'application. Notez que la ligne se termine par une accolade ouvrante, indiquant le début d'un nouveau bloc de code, et qu'il y a l'accolade fermante correspondante quatre lignes plus bas. Tout ce qui se trouve à l'intérieur de ces accolades sera exécuté à chaque itération de la boucle. Nous pourrions traduire cette ligne par "traite `items` comme une collection de chaînes de caractères, puis extrais chacune de ces chaînes en lui donnant le nom `item`, puis exécute le code suivant…"
- La ligne `if item.hasPrefix("nssl") {` est la première ligne de notre boucle. À ce stade, nous sommes prêts à travailler avec le nom du premier fichier qui a été assigné à `item`. Pour déterminer s'il s'agit d'un nom qui nous intéresse, nous utilisons la méthode `hasPrefix()` : elle prend un paramètre (le préfixe à rechercher) et retourne true ou false. Le "if" du début indique que cette ligne est une instruction conditionnelle : si l'élément a le préfixe "nssl", alors ... c'est vrai, et une autre accolade ouvrante marque un nouvel autre bloc de code. Cette fois, le code ne sera exécuté que si `hasPrefix()` a retourné la valeur true.
- Enfin, la ligne `// this is a picture to load!` (ceci est une image à charger!) est un commentaire - si nous arrivons jusqu'ici, `item` contient le nom d'une image à charger depuis notre paquet, nous devons donc le stocker quelque part.

Ces quelques lignes de code contiennent beaucoup de notions importantes, donc avant de continuer, récapitulons :

- Nous utilisons `let` pour déclarer des constantes. Les constantes correspondent à des données que nous souhaitons référencer, tout en sachant que leur valeur ne sera pas modifiée. Par exemple, votre date de naissance est une constante, mais votre âge ne l’est pas - votre âge est une variable, car il change tous les ans.
- Les programmeurs en Swift aiment vraiment utiliser des constantes là où la plupart des développeurs utilisent des variables. En effet, en programmant, vous allez commencer à vous rendre compte que la plupart des données que vous stockez ne changent pas beaucoup, alors vous pouvez également les déclarer comme des constantes. Cela permet au système de faire fonctionner votre code plus rapidement et ajoute également une sécurité supplémentaire car si vous essayez de modifier une constante, Xcode refusera de compiler votre application.
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
