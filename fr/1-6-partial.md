# Touches finales : peauxBarsOnTap, marges de la zone de sécurité

À ce stade, votre projet fonctionne parfaitement : vous pouvez appuyer sur Cmd + R pour l’exécuter, parcourir rapidement les images du tableau, puis appuyer sur l’une d'entre elles pour l’afficher. Mais avant de terminer ce projet, nous allons effectuer plusieurs autres petits changements qui rendent le résultat final un peu plus raffiné.

Tout d'abord, vous avez peut-être remarqué que toutes les images sont étirées pour remplir l'écran. Ce n'est pas un accident - c'est le paramètre par défaut de `UIImageView`.

Quelques clics suffisent pour résoudre ce problème : choisissez Main.storyboard, sélectionnez l'élément Image View dans Detail View controller, puis choisissez l'onglet Attributes Inspector dans le volet de droite - il s'agit du quatrième onglet, juste à gauche de l'icône de la règle.

Si vous n’avez pas envie de le chercher, appuyez simplement sur Cmd + Alt + 4 pour l’afficher. L'étirement est provoqué par le mode d'affichage, qui est un menu déroulant dont la valeur par défaut est "Scale to Fill" (remplissage à l'échelle). Changez-le en "Aspect Fit" (Ajuster le format) et ce premier problème est résolu.

![Le mode d'affichage "Aspect Fit" pour les éléments UIImageView les oblige à redimensionner leurs images pour les rendre entièrement visibles.](1-18.png)

Si vous vous posez la question, "Aspect Fit" redimensionne l'image pour qu'elle soit entièrement visible. Il y a aussi "Aspect Fill", qui redimensionne l'image de manière à ne pas laisser d'espace vide. Cela signifie généralement le rognage de la largeur ou de la hauteur. Si vous utilisez "Aspect Fill" (Remplir en conservant l’aspect), l’image déborde en dehors de la zone de visualisation. Veillez donc à activer "Clip to Bounds" pour éviter ceci.

Le deuxième changement que nous allons effectuer consiste à permettre aux utilisateurs de visualiser les images en plein écran, sans aucune barre de navigation. Il existe un moyen vraiment très simple d'y parvenir, et il s'agit d'une propriété sur `UINavigationController` appelée` hidesBarsOnTap`. Lorsque celle-ci est définie sur true, l'utilisateur peut appuyer n'importe où sur le contrôleur de vue actuel pour masquer la barre de navigation, puis appuyer de nouveau pour l'afficher.

Mise en garde : vous devez la configurer avec soin lorsque vous travaillez avec un iPhone. Si nous l'avions tout le temps activée, cela aurait également affecté les taps sur la vue affichant le tableau, provoquant la pagaille lorsque l'utilisateur tente de sélectionner des éléments. Nous ne devons donc l'activer que lorsque Detail View Controller est affiché, puis la désactiver lorsqu'il est caché.

Vous avez déjà rencontré la méthode `viewDidLoad()`, appelée lorsque la mise en page du contrôleur de vue a été chargée. Il y en a plusieurs autres qui sont appelés lorsque la vue est sur le point d'être affichée, quand elle a vient d'être affichée, quand elle va disparaître et quand elle a disparu. Celles-ci sont appelées respectivement `viewWillAppear()`, `viewDidAppear()`, `viewWillDisappear()` et `viewDidDisappear()`. Nous allons utiliser `viewWillAppear()` et `viewWillDisappear()` pour modifier la propriété `hidesBarsOnTap` afin qu'elle ne soit définie sur true que lorsque Detail View Controller est affiché.

Ouvrez DetailViewController.swift, puis ajoutez ces deux nouvelles méthodes directement à la suite de la méthode `viewDidLoad()` :

    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        navigationController?.hidesBarsOnTap = true
    }

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        navigationController?.hidesBarsOnTap = false
    }

Il y a quelques points importants à noter ici:

- Nous utilisons le mot-clé override pour chacune de ces méthodes, car elles ont déjà des valeurs par défaut définies dans `UIViewController` et nous lui demandons d'utiliser les nôtres à la place. Ne vous inquiétez pas si vous ne savez pas quand utiliser ou non override, car si vous ne l'utilisez pas et que c'est obligatoire, Xcode vous le dira.
- Les deux méthodes ont un seul paramètre : que l'action soit animée ou non. Nous ne nous en soucions pas vraiment dans ce cas, alors ignorons le.
- Les deux méthodes utilisent à nouveau le préfixe `super` : ` super.viewWillAppear()` et `super.viewWillDisappear()`. Cela signifie "indique à mon type de données parent que ces méthodes ont été appelées". Dans ce cas, cela signifie qu'il transmet la méthode à `UIViewController`, qui peut effectuer son propre traitement.
- Nous utilisons à nouveau la propriété `navigationController`, ce qui fonctionnera bien car nous avons été placés sur la pile du contrôleur de navigation à partir de `ViewController`. Nous accédons à la propriété à l’aide du "?". Par conséquent, si nous *ne sommes pas* dans un contrôleur de navigation, les lignes `hidesBarsOnTap` ne feront rien.

Si vous exécutez l'application maintenant, vous verrez que vous pouvez appuyer sur le nom d'une image pour l'afficher à sa taille d'origine, et qu'elle ne sera plus étirée. Lorsque vous visualisez une image, vous pouvez appuyer n'importe où à l'écran pour masquer la barre de navigation en haut, puis taper pour l'afficher à nouveau.

The third change is a small but important one. If you look at other apps that use table views and navigation controllers to display screens (again, Settings is great for this), you might notice gray arrows at the right of the table view cells. This is called a disclosure indicator, and it’s a subtle user interface hint that tapping this row will show more information.

It only takes a few clicks in Interface Builder to get this disclosure arrow in our table view. Open Main.storyboard, then click on the table view cell – that’s the one that says “Title”, directly below “Prototype Cells”. The table view contains a cell, the cell contains a content view, and the content view contains a label called “Title” so it’s easy to select the wrong thing. As a result, you’re likely to find it easiest to use the document outline to select exactly the right thing – you want to select the thing marked “Picture”, which is the reuse identifier we attached to our table view cell.

When that’s selected, you should be able go to the attributes inspector and see “Style: Basic”, “Identifier: Picture”, and so on. You will also see “Accessory: None” – please change that to “Disclosure Indicator”, which will cause the gray arrow to show.

The fourth is small but important: we’re going to place some text in the gray bar at the top. You’ve already seen that view controllers have `storyboard` and `navigationController` properties that we get from `UIViewController`. Well, they also have a `title` property that automatically gets read by navigation controller: if you provide this title, it will be displayed in the gray navigation bar at the top.

In `ViewController`, add this code to `viewDidLoad()` after the call to `super.viewDidLoad()`:

    title = "Storm Viewer"

This title is also automatically used for the “Back” button, so that users know what they are going back to.

In `DetailViewController` we *could* add something like this to `viewDidLoad()`:

    title = "View Picture"

That would work fine, but instead we’re going to use some dynamic text: we’re going to display the name of the selected picture instead.

Add this to `viewDidLoad()` in `DetailViewController`:

    title = selectedImage

We don’t need to unwrap `selectedImage` here because both `selectedImage` and `title` are optional strings – we’re assigning one optional string to another. `title` is optional because it’s nil by default: view controllers have no title, thus showing no text in the navigation bar.


## Large titles in iOS 11

This is an entirely optional change, but I wanted to introduce it to you nice and early so you can try it for yourself and see what you think.

iOS 11 revamped Apple’s design guidelines in many places, but the most noticeable is the use of *large titles* – the text that appears in the gray bar at the top of apps. The default style is small text, which is what we’ve had so far, but with a couple of lines of code we can adopt the new design.

First, add this to `viewDidLoad()` in ViewController.swift:

    navigationController?.navigationBar.prefersLargeTitles = true

That enables large titles across our app, and you’ll see an immediate difference: “Storm Viewer” becomes much bigger, and in the detail view controller all the image titles are also big. You’ll notice the title is no longer static either – if you pull down gently you’ll see it stretches ever so slightly, and if you try scrolling up in our table view you’ll see the titles shrinks away.

Apple recommends you use large titles only when it makes sense, and that usually means only on the first screen of your app. As you’ve seen, the default behavior when enabled is to have large titles everywhere, but that’s because each new view controller that pushed onto the navigation controller stack inherits the style of its predecessor.

In this app we want “Storm Viewer” to appear big, but the detail screen to look normal. To make that happen we need to add a line of code to `viewDidLoad()` in DetailViewController.swift:

    navigationItem.largeTitleDisplayMode = .never

That’s all it takes – the large titles should behave properly now.


## And what about iPhone X?

iPhone X introduced the first iPhone screen that isn’t rectangular, which creates some interesting problems. In particular, the rounded corners of the screen mean that content can easily be clipped if it isn’t positioned correctly, so the system tries to adapt by making *safe areas* – parts of the screen that are automatically avoided so that clipping doesn’t happen.

In this app, our table view controller will automatically run edge to edge because Apple has done all the hard work for us, but the image in our detail view controller won’t – it will have a white gap at the bottom to leave space for the home indicator.

That is sometimes what you’ll want, but here it looks pretty poor. Fortunately, it’s trivial to fix: open Main.storyboard, select the view inside Detail View Controller, then uncheck the Safe Area Layout Guide box in the size inspector. That will ask the view (and all its subviews) to run edge to edge, which looks better.

While we’re talking about the iPhone X and other notch-equipped devices, let’s extend our use of `hidesBarsOnTap` so that the home indicator gets hidden too. This is controlled by overriding a computed property called `prefersHomeIndicatorAutoHidden` – if that returns true, the home indicator will disappear after a couple of seconds, only to automatically reappear when the user touches the screen.

We’re going to write this method so that it returns the value of our navigation controller’s `hidesBarsOnTap` property, meaning that the bars and the home indicator should disappear or reappear together.

We should in theory always have a navigation controller so we could probably force unwrap it and read its property. However a better idea is to use it optionally and provide `false` as the default value if the navigation controller isn’t present. Add this property to DetailViewController.swift now:

    override var prefersHomeIndicatorAutoHidden: Bool {
        return navigationController?.hidesBarsOnTap ?? false
    }

**Note:** The `??` operator is called the *nil coalescing operator*, and in this case it means “if the navigation controller doesn’t exist, send back `false` rather than trying to read its `hidesBarsOnTap` property.”

That property gets checked when the view controller is first shown, but will also automatically get called whenever the bars get toggled using `hidesBarsOnTap`. So, that’s our work done: the home indicator should automatically disappear a few seconds after the bars disappear.

That’s the last of the changes – we’re done! Go ahead and run the project now and admire your handiwork.
