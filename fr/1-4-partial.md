# Réaliser un écran affichant l'image

À ce stade de notre application, nous avons une liste d'images à choisir, mais bien que nous puissions appuyer dessus, il ne se passe rien. Notre prochain objectif est de concevoir un nouvel écran qui apparaîtra lorsque l'utilisateur appuiera sur une ligne. Nous allons lui faire afficher en plein écran la photo sélectionnée, qui glissera automatiquement lorsqu'on tappe sur le nom de l'image.

Cette tâche peut être divisée en deux tâches plus petites. Premièrement, nous devons créer du nouveau code qui sera responsable de cet écran. Deuxièmement, nous devons dessiner l'interface utilisateur de cet écran dans Interface Builder.

Commençons par la partie facile : créer le nouveau code respondable de l’écran affichant l'image. Dans la barre de menus, cliquez sur File puis New > File. Une fenêtre contenant de nombreuses options apparaît. Dans cette liste, choisissez iOS > Cocoa Touch Class, puis cliquez sur Suivant.

Vous serez invité à donner un nom au nouvel écran et à indiquer à iOS de quoi il doit hériter. Veuillez entrer "DetailViewController" pour son nom dans le champ Class et "UIViewController" pour "Subclass of". Assurez-vous que "Also create XIB file" n'est pas sélectionné, puis cliquez sur Next et Create pour ajouter le nouveau fichier.

Voilà, le premier travail est effectué - nous avons un nouveau fichier qui contiendra du code pour l’écran affichant l'image.

La deuxième tâche demande un peu plus de réflexion. Retournez dans Main.storyboard, et vous verrez nos deux contrôleurs de vue existants : le Navigation View Controller à gauche et le Table View Controller à droite. Nous allons maintenant ajouter un nouveau contrôleur de vue, un nouvel écran, qui sera notre écran chargé d'afficher l'image.

Tout d’abord, ouvrez la bibliothèque d’objets et recherchez "View Controller". Faites-le glisser dans l'espace situé à droite de votre contrôleur de vue existant. Vous pouvez vraiment le placer n’importe où, mais c’est bien d’organiser vos écrans pour qu’ils se suivent logiquement de gauche à droite.

Maintenant, si vous regardez dans Document Outline (structure du document), vous verrez apparaître une deuxième scène : "View Controller Scene", il y en a donc une pour la vue contenant le tableau avec le nom des images et une autre pour la vue affichant l'image. Si vous ne savez pas laquelle est laquelle, cliquez simplement sur le nouvel écran - dans le grand espace vide blanc qui vient d'être créé - et il devrait sélectionner la bonne scène dans le volet Document Outline.

Lorsque nous avions précédemment créé notre cellule, nous lui avions attribué un identifiant afin de pouvoir l'utiliser dans le code. Nous devons faire la même chose pour ce nouvel écran. Lorsque vous l'avez sélectionné il y a quelques instants, il a dû mettre en surbrillance "View" dans le volet Document Outline. Au-dessus se trouve "View Controller" avec une icône jaune à côté de celui-ci - veuillez cliquer maintenant dessus pour sélectionner le contrôleur de vue complet.

Pour donner un nom à ce contrôleur de vue, accédez à l'onglet Identity Inspector dans le volet de droite en appuyant sur Cmd + Alt + 3 ou en utilisant le menu. Maintenant, entrez "Détail" dans le champ "Storyboard ID". C’est tout, nous pouvons maintenant appeler ce contrôleur de vue "Détail" dans le code. Tant que vous y êtes, cliquez sur la flèche de la liste déroulante de Class, puis sélectionnez "DetailViewController" pour que notre interface utilisateur soit connectée au nouveau code créé précédemment.

Passons maintenant à la partie intéressante : nous voulons que cet écran affiche l’image sélectionnée par l’utilisateur, belle et grande, nous devons donc utiliser un nouveau composant d’interface utilisateur appelé `UIImageView`. Comme vous pouvez le voir à partir du nom, celui-ci fait partie de UIKit (d'où le "UI"), et est responsable de l'affichage des images - parfait !

Regardez dans la bibliothèque d'objets pour trouver Image View ; vous trouverez peut-être qu'il est plus facile de réutiliser le champ de recherche. Cliquez et faites glisser le composant Image View de la bibliothèque d'objets sur l'écran chargé d'affiché l'image, puis relâchez-le. Maintenant, faites glisser ses bords pour qu'ils remplissent tout l'écran'.

![Faites glisser les poignées présentes sur les bords de l'Image View pour qu'il occupe tout l'écran, y compris sous l'icône de la batterie du simulateur.](1-21.png)

This image view has no content right now, so it's filled with a pale blue background and the word `UIImageView`. We won't be assigning any content to it right now, though – that's something we'll do when the program runs. Instead, we need to tell the image view how to size itself for our screen, whether that's iPhone or iPad.

This might seem strange at first, after all you just placed it to fill the view controller, and it has the same size as the view controller, so that should be it, right? Well, not quite. Think about it: there are lots of iOS devices your app might run on, all with different sizes. So, how should the image view respond when it’s being shown on a 6 Plus or perhaps even an iPad?

iOS has an answer for this. And it's a brilliant answer that in many ways works like magic to do what you want. It's called Auto Layout: it lets you define rules for how your views should be laid out, and it automatically makes sure those rules are followed.

But – and this is a big but! – it has two rules of its own, both of which must be followed by you:

- Your layout rules must be complete. That is, you can't specify only an X position for something, you must also specify a Y position. If it's been a while since you were at school, "X" is position from the left of the screen, and "Y" is position from the top of the screen.
- Your layout rules must not conflict. That is, you can't specify that a view must be 10 points away from the left edge, 10 points away from the right edge, and 1000 points wide. An iPhone 5 screen is only 320 points wide, so your layout is mathematically impossible. Auto Layout will try to recover from these problems by breaking rules until it finds a solution, but the end result is never what you want.

You can create Auto Layout rules – known as *constraints* – entirely inside Interface Builder, and it will warn you if you aren't following the two rules. It will even help you correct any mistakes you make by suggesting fixes. Note: the fixes it suggests *might* be correct, but they might not be – tread carefully!

We're going to create four constraints now: one each for the top, bottom, left and right of the image view so that it expands to fill the detail view controller regardless of its size. There are lots of ways of adding Auto Layout constraints, but the easiest way right now is to select the image view then go to the Editor menu and choose > Resolve Auto Layout Issues > Reset To Suggested Constraints.

You’ll see that option listed twice in the menu because there are two subtly different options, but in this instance it doesn’t matter which one you choose. If you prefer keyboard shortcuts, press Shift+Alt+Cmd+= to accomplish the same thing.

Visually, your layout will look pretty much identical once you've added the constraints, but there are two subtle differences. First, there's a thin blue line surrounding the `UIImageView` on the detail view controller, which is Interface Builder's way of showing you that the image view has a correct Auto Layout definition.

Second, in the document outline pane you'll see a new entry for "Constraints" beneath the image view. All four constraints that were added are hidden under that Constraints item, and you can expand it to view them individually if you’re curious.

With the constraints added, there's one more thing to do here before we're finished with Interface Builder, and that's to connect our new image view to some code. You see, having the image view inside the layout isn't enough – if we actually want to *use* the image view inside code, we need to create a property for it that's attached to the layout.

This property is like the `pictures` array we made previously, but it has a little bit more “interesting” Swift syntax we need to cover. Even more cunningly, it’s created using a really bizarre piece of user interface design that will send your brain  for a loop if you’ve used other graphical IDEs.

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
