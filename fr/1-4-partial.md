# Concevoir un écran affichant l'image

À ce stade de notre application, nous avons une liste d'images à choisir, mais bien que nous puissions appuyer sur leur nom, il ne se passe rien. Notre prochain objectif est de concevoir un nouvel écran qui apparaîtra lorsque l'utilisateur appuiera sur une ligne. Nous allons lui faire afficher en plein écran la photo sélectionnée, qui apparaîtra en glissant automatiquement lorsqu'on tappe sur le nom de l'image.

Cette tâche peut être divisée en deux tâches plus petites. Premièrement, nous devons créer du nouveau code qui sera responsable de cet écran. Deuxièmement, nous devons dessiner l'interface utilisateur de cet écran dans Interface Builder.

Commençons par la partie facile : créer le nouveau code respondable de l’écran affichant l'image. Dans la barre de menus, cliquez sur File puis New > File. Une fenêtre contenant de nombreuses options apparaît. Dans cette liste, choisissez iOS > Cocoa Touch Class, puis cliquez sur Suivant.

Vous serez invité à donner un nom au nouvel écran et à indiquer à iOS de quoi il doit hériter. Veuillez entrer "DetailViewController" pour son nom dans le champ Class et "UIViewController" pour "Subclass of". Assurez-vous que "Also create XIB file" n'est pas sélectionné, puis cliquez sur Next et Create pour ajouter le nouveau fichier.

Voilà, le premier travail est effectué - nous avons un nouveau fichier qui contiendra du code pour l’écran affichant l'image.

La deuxième tâche demande un peu plus de réflexion. Retournez dans Main.storyboard, et vous verrez nos deux contrôleurs de vue existants : le Navigation View Controller à gauche et le Table View Controller à droite. Nous allons maintenant ajouter un nouveau contrôleur de vue, un nouvel écran, qui sera notre écran chargé affichant l'image.

Tout d’abord, ouvrez la bibliothèque d’objets et recherchez "View Controller". Faites-le glisser dans l'espace situé à droite de vos contrôleurs de vue existants. Vous pouvez vraiment le placer n’importe où, mais c’est bien d’organiser vos écrans pour qu’ils se suivent logiquement de gauche à droite.

Maintenant, si vous regardez dans le volet Document Outline (structure du document), vous verrez apparaître une deuxième scène : "View Controller Scene", il y en a donc une pour la vue contenant le tableau avec le nom des images et une autre pour la vue qui affichera l'image. Si vous ne savez pas laquelle est laquelle, cliquez simplement sur le nouvel écran - dans le grand espace vide blanc qui vient d'être créé - et il devrait sélectionner la bonne scène dans le volet Document Outline.

Lorsque nous avions précédemment créé notre cellule, nous lui avions attribué un identifiant afin de pouvoir l'utiliser dans le code. Nous devons faire la même chose pour ce nouvel écran. Lorsque vous l'avez sélectionné il y a quelques instants, il a dû mettre en surbrillance "View" dans le volet Document Outline. Au-dessus se trouve "View Controller" avec une icône jaune à côté de celui-ci - veuillez cliquer maintenant dessus pour sélectionner le contrôleur de vue complet.

Pour donner un nom à ce contrôleur de vue, accédez à l'onglet Identity Inspector dans le volet de droite en appuyant sur Cmd + Alt + 3 ou en utilisant le menu. Maintenant, entrez "Détail" dans le champ "Storyboard ID". C’est tout, nous pouvons maintenant appeler ce contrôleur de vue "Détail" dans le code. Tant que vous y êtes, cliquez sur la flèche de la liste déroulante de Class, puis sélectionnez "DetailViewController" pour que notre interface utilisateur soit connectée au nouveau code créé précédemment.

Passons maintenant à la partie intéressante : nous voulons que cet écran affiche l’image sélectionnée par l’utilisateur, belle et grande, nous devons donc utiliser un nouveau composant d’interface utilisateur appelé `UIImageView`. Comme vous pouvez le voir à partir du nom, celui-ci fait partie de UIKit (d'où le "UI"), et est responsable de l'affichage des images - parfait !

Regardez dans la bibliothèque d'objets pour trouver Image View ; vous trouverez peut-être qu'il est plus facile de réutiliser le champ de recherche. Cliquez et faites glisser le composant Image View de la bibliothèque d'objets sur l'écran chargé d'affiché l'image, puis relâchez-le. Maintenant, faites glisser ses bords pour qu'ils remplissent tout l'écran'.

![Faites glisser les poignées présentes sur les bords de l'Image View pour qu'il occupe tout l'écran, y compris sous l'icône de la batterie du simulateur.](1-21.png)

Le composant Image View n'a pas de contenu pour le moment, il est donc rempli d'un fond bleu pâle et du mot `UIImageView`. Nous ne lui attribuerons aucun contenu pour le moment, car c’est quelque chose que nous ferons lors de l’exécution du programme. Au lieu de cela, nous devons indiquer à Image View  comment se dimensionner tout seul pour notre écran, qu'il s'agisse d'un iPhone ou d'un iPad.

Cela peut sembler étrange au début, après tout vous venez de le placer pour qu'il remplisse entièrement l'écran, et il a la même taille que le contrôleur de vue, donc ça devrait être suffisant, non? Et bien pas tout à fait. Pensez-y : votre application peut fonctionner sur de nombreux appareils fonctionnanr sous iOS, tous de tailles différentes. Comment l’image doit-elle réagir lorsqu’elle est affichée sur un iPhone 6 Plus ou même sur un iPad?

iOS a une réponse à cela. Et c'est une réponse brillante qui, à bien des égards, fonctionne comme par magie pour faire ce que vous voulez. Elle s'appelle Auto Layout (mise en page automatique) : elle vous permet de définir des règles pour la mise en page de vos vues et garantit automatiquement que ces règles sont respectées.

Mais - et c'est un gros mais ! - il y a deux règles que vous devez toutes les deux respecter :

- Vos règles de mise en page doivent être complètes. Autrement dit, vous ne pouvez pas spécifier uniquement une position X pour quelque chose, vous devez également spécifier une position Y. Si cela fait longtemps que vous n'êtes plus à l'école, "X" est la position à partir du bord gauche de l'écran et "Y" la position à partir du haut de l'écran.
- Vos règles de mise en page ne doivent pas rentrer en conflit. En d'autres termes, vous ne pouvez pas spécifier qu'une vue doit se situer à 10 points du bord gauche, 10 points du bord droit et fasse 1 000 points de large. L'écran de l'iPhone 5 ne fait que 320 points de large, ce qui rend votre mise en page impossible d'un point de vue mathématique. Auto Layout essaiera de remédier à ces problèmes en enfreignant les règles jusqu'à ce qu'il trouve une solution, mais le résultat final n'est jamais ce que vous voulez.

Vous pouvez créer des règles pour Auto Layout - appelées *contraintes* - entièrement dans Interface Builder. Ce dernier vous avertira si vous ne respectez pas les deux règles. Il vous aidera même à corriger vos erreurs en vous suggérant des corrections. Remarque: les corrections suggérées *peuvent* être corrects, ou ne pas l'être - allez-y prudemment !

Nous allons maintenant créer quatre contraintes pour Image View : une pour le haut, une pour le bas, une pour la gauche et une autre pour la droite afin qu'Image View s'agrandisse de manière à remplir tout l'écran, quelle que soit sa taille. Il existe de nombreuses façons d’ajouter des contraintes pour Auto Layout, mais le moyen le plus simple pour le moment consiste à sélectionner Image View, puis d'aller dans la barre des menus et cliquer sur Editor > Resolve Auto Layout Issues > Reset To Suggested Constraints.

Cette option est affichée deux fois dans le menu car il existe deux options légèrement différentes, mais dans ce cas, peu importe l’option choisie. Si vous préférez les raccourcis clavier, appuyez sur Maj + Alt + Cmd + = pour effectuer la même chose.

Visuellement, votre mise en page sera pratiquement identique une fois que vous aurez ajouté les contraintes, mais il existe deux différences subtiles. Premièrement, une fine ligne bleue entoure le composant `UIImageView` dans le View Controller, qui est la façon pour Interface Builder de vous montrer que les règles pour Image View sont correctes.

Deuxièmement, dans le volet Document Outline, vous verrez une nouvelle entrée "Constraints" sous Image View. Les quatre contraintes qui ont été ajoutées sont masquées sous cet élément "Constraints". Si vous êtes curieux, vous pouvez le développer pour afficher les contraintes individuellement.

Avec les contraintes ajoutées, il reste encore une chose à faire avant d’avoir terminé avec Interface Builder : connecter Image View à du code. Vous voyez, avoir un composant Image View dans le contrôleur de vue ne suffit pas - si nous voulons en fait *utiliser* Image View dans le code, nous devons créer une propriété dans le code qui est attachée au composant dans le storyboard.

Cette propriété est comme le tableau `pictures` que nous avons créé précédemment, mais elle a une syntaxe un peu plus "intéressante" en Swift que nous devons expliquer. Encore plus astucieux, elle est créée à l’aide d’un outil de conception d’interface utilisateur vraiment bizarre qui va faire chauffer votre cerveau si vous avez utilisé d’autres environnements de développement graphiques (IDE).

Let’s dive in, and I’ll explain on the way. Xcode has a special display layout called the Assistant Editor, which splits your Xcode editor in two: the view you had before on top, and a related view at the bottom. In this case, it's going to show us Interface Builder on top, and the code for the detail view controller below.

Xcode decides what code to show based on what item is selected in Interface Builder, so make sure the image view is still selected and choose View > Assistant Editor > Show Assistant Editor from the menu. You can also use the keyboard shortcut Alt+Cmd+Return if you prefer.

Xcode can display the assistant editor as two vertical panes rather than two horizontal panes. I find the horizontal panes easiest – i.e., one above the other. You can switch between them by going to View > Assistant Editor and choosing either Assistant Editors On Right or Assistant Editors on Bottom.

Regardless of which you prefer, you should now see the detail view controller in Interface Builder in one pane, and in the other pane the source code for DetailViewController.swift. Xcode knows to load DetailViewController.swift because you changed the class for this screen to be “DetailViewController” just after you changed its storyboard ID.

Now for the bizarre piece of UI. What I want you to do is this:

1. Make sure the image view is selected.
2. Hold down the Ctrl key on your keyboard.
3. Press your mouse button down on the image view, but hold it down – don’t release it.
4. While continuing to hold down Ctrl and your mouse button, drag from the image view into your code – into the other assistant editor pane.
5. As you move your mouse cursor, you should see a blue line stretch out from the image view into your code. Stretch that line so that it points between `class DetailViewController: UIViewController {` and `override func viewDidLoad() {`.
6. When you’re between those two, a horizontal blue line should appear, along with a tooltip saying Insert Outlet Or Outlet Connection. When you see that, let go of both Ctrl and your mouse button. (It doesn’t matter which one you release first.)

If you follow those steps, a balloon should appear with five fields: Connection, Object, Name, Type, and Storage.

![When you Ctrl-drag from Interface Builder into your code, a bubble will appear offering to create an outlet for you.](1-22.png)

By default the options should be “Outlet” for connection, “Detail View Controller” for Object, nothing for name, “UIImageView” for type, and “Strong” for storage. If you see “Weak” for storage please change it to “Strong” – Xcode will remember that setting from now on.

Leave all of them alone except for Name – I’d like you to enter “imageView” in there. When you’ve done that click the Connect button, and Xcode will insert a line of code into DetailViewController.swift. You should see this:

    class DetailViewController: UIViewController {
        @IBOutlet var imageView: UIImageView!

        override func viewDidLoad() {
            super.viewDidLoad()

To the left of the new line of code, in the gutter next to the line number, is a gray circle with a line around it. If you move your mouse cursor over that you’ll see the image view flash – that little circle is Xcode’s way of telling you the line of code is connected to the image view in your storyboard.

So, we Ctrl-dragged from Interface Builder straight into our Swift file, and Xcode wrote a line of code for us as a result. Some bits of that code are new, so let's break down the whole line:

- `@IBOutlet`: This attribute is used to tell Xcode that there's a connection between this line of code and Interface Builder.
- `var`: This declares a new variable or variable property.
- `imageView`: This was the property name assigned to the `UIImageView`. Note the way capital letters are used: variables and constants should start with a lowercase letter, then use a capital letter at the start of any subsequent words. For example, `myAwesomeVariable`. This is sometimes called camel case.
- `UIImageView!`: This declares the property to be of type `UIImageView`, and again we see the implicitly unwrapped optional symbol: `!`. This means that that `UIImageView` may be there or it may not be there, but we're certain it definitely will be there by the time we want to use it.

If you were struggling to understand implicitly unwrapped optionals (don't worry; they are complicated!), this code might make it a bit clearer. You see, when the detail view controller is being created, its view hasn't been loaded yet – it's just some code running on the CPU.

When the basic stuff has been done (allocating enough memory to hold it all, for example), iOS goes ahead and loads the layout from the storyboard, then connects all the outlets from the storyboard to the code.

So, when the detail controller is first made, the `UIImageView` doesn't exist because it hasn't been created yet – but we still need to have some space for it in memory. At this point, the property is `nil`, or just some empty memory. But when the view gets loaded and the outlet gets connected, the `UIImageView` will point to a real `UIImageView`, not to `nil`, so we can start using it.

In short: it starts life as `nil`, then gets set to a value before we use it, so we're certain it won't ever be `nil` by the time we want to use it – a textbook case of implicitly unwrapped optionals. If you still don't understand implicitly unwrapped optionals, that's perfectly fine – keep on going and they'll become clear over time.

That’s our detail screen complete – we’re done with Interface Builder for now, and can return to code. This also means we’re done with the assistant editor, so you can return to the full-screen editor by going to View > Standard Editor > Show Standard Editor.
