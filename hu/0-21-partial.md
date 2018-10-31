# Closures [^closure]

You've met integers, strings, doubles, floats, Booleans, arrays, dictionaries, structs and classes so far, but there's another type of data that is used extensively in Swift, and it's called a closure. These are complicated, but they are so powerful and expressive that they are used pervasively in Cocoa Touch, so you won't get very far without understanding them.

A closure can be thought of as a variable that holds code. So, where an integer holds 0 or 500, a closure holds lines of Swift code. It's different to a function, though, because closures are a data type in their own right: you can pass a closure as a parameter or store it as a property. Closures also capture the environment where they are created, which means they take a copy of the values that are used inside them.

You never *need* to design your own closures so don't be afraid if you find the following quite complicated. However, both Cocoa and Cocoa Touch will often ask you to write closures to match their needs, so you at least need to know how they work. Let's take a Cocoa Touch example first:

```swift
let vw = UIView()

UIView.animate(withDuration: 0.5, animations: {
    vw.alpha = 0
})
```

`UIView` egy iOS adat típus a UIKit részeként, a legalapvetőbb felhasználói felületi konkténert testesíti meg. Amiatt ne aggódj, hogy mi a szerepe, ami jelenleg számít az az, hogy ez egy alapvető UI komponens. A `UIView`-nak van egy `animate()` metódusa, mely képes animálva megváltoztatni az interfész megjelenését - te leírod mi változzon és mennyi idő alatt, a Cocoa Touch pedig elvégzi a többit.

Az `animate()` metódus két paramétert vár: hány másodperc alatt szeretnéd az animációt végrehajtani és a funkcionális blokkot, amelyben a végrehajtandó animációs rész található. Én fél másodpercet határoztam meg első paraméterként, másodikként pedig megkértem a UIKit-et, hogy változtassa a konténer alfáját (ez az átlátszatlansága) 0-ra, ami azt jelenti, hogy "teljesen átlátszó" lesz.

This method needs to use a closure because UIKit has to do all sorts of work to prepare for the animation to begin, so what happens is that UIKit takes a copy of the code inside the braces (that's our closure), stores it away, does all its prep work, then runs our code when it's ready. This wouldn't be possible if we just run our code directly.

The above code also shows how closures capture their environment: I declared the `vw` constant outside of the closure, then used it inside. Swift detects this, and makes that data available inside the closure too.

Swift's system of automatically capturing a closure's environment is very helpful, but can occasionally trip you up: if object A stores a closure as a property, and that property also references object A, you have something called a strong reference cycle and you'll have unhappy users. This is a substantially more advanced topic than you need to know right now, so don't worry too much about it just yet.


## Trailing closures

As closures are used so frequently, Swift can apply a little syntactic sugar to make your code easier to read. The rule is this: if the last parameter to a method takes a closure, you can eliminate that parameter and instead provide it as a block of code inside braces. For example, we could convert the previous code to this:

```swift
let vw = UIView()

UIView.animate(withDuration: 0.5) {
    vw.alpha = 0
}
```

[^closure]: It does make your code shorter and easier to read, so this syntax form – known as trailing closure syntax – is preferred. Closures are self-contained blocks of functionality that can be passed around and used in your code. Closures in Swift are similar to blocks in C and Objective-C and to lambdas in other programming languages.
