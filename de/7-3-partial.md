# JSON parsen mit dem Codable-Protokoll

JSON - kurz für JavaScript Object Notation - ist eine Art, Daten zu beschreiben. Es ist nicht gerade das lesbarste Format, aber es ist kompakt und kann leicht von Computern geparst werden, was es online beliebt macht, wo Bandbreite kostbar ist.

Bevor wir uns ans Parsen machen, hier ein winziger Ausschnitt der tatsächlichen JSON, welche du empfangen wirst:

    {
        "metadata":{
            "responseInfo":{
                "status":200,
                "developerMessage":"OK",
            }
        },
        "results":[
            {
                "title":"Legal immigrants should get freedom before undocumented immigrants – moral, just and fair",
                "body":"I am petitioning President Trump's Administration to take a humane view of the plight of legal immigrants. Specifically, legal immigrants in Employment Based (EB) category. I believe, such immigrants were short changed in the recently announced reforms via Executive Action (EA), which was otherwise long due and a welcome announcement.",
                "issues":[
                    {
                        "id":"28",
                        "name":"Human Rights"
                    },
                    {
                        "id":"29",
                        "name":"Immigration"
                    }
                ],
                "signatureThreshold":100000,
                "signatureCount":267,
                "signaturesNeeded":99733,
            },
            {
                "title":"National database for police shootings.",
                "body":"There is no reliable national data on how many people are shot by police officers each year. In signing this petition, I am urging the President to bring an end to this absence of visibility by creating a federally controlled, publicly accessible database of officer-involved shootings.",
                "issues":[
                    {
                        "id":"28",
                        "name":"Human Rights"
                    }
                ],
                "signatureThreshold":100000,
                "signatureCount":17453,
                "signaturesNeeded":82547,
            }
        ]
    }

Tatsächlich wirst du 2000-3000 Zeilen von diesem Zeug kriegen, die Petitionen von US-Bürgern über alle möglichen politischen Dinge enthalten. Es ist eigentlich gar nicht wichtig (für uns), um was es bei den Petitionen geht, wir interessieren uns nur für die Datenstruktur. Insbesondere:

1. Es gibt einen Metadaten-Wert, der einen Wert namens `responseInfo` enthält, der wiederum einen Status-Wert enthält. Mit Status 200 meinen Internet-Entwickler, dass "alles OK" ist.
2. Es gibt einen Resultat-Wert, der eine Reihe von Petitionen enthält.
3. Jede Petition enthält einen Titel, einen Textkörper, einige Sachverhalte, zu denen sie Bezug hat, und Informationen zu den Unterschriften.
4. JSON hat auch Strings und Integers (Ganzzahlen). Beachte, wie alle Strings von Anführungszeichen umgeben sind, die Ganzzahlen aber nicht.

Swift hat eine eingebaute Unterstützung für JSON mit einem Protokoll namens `Codable`. Wenn du sagst, "meine Daten genügen `Codable`", wird Swift dir erlauben, mit nur wenig Code frei zwischen diesen Daten und JSON zu konvertieren.

Swifts einfache Typen wie `String` und `Int` genügen automatisch `Codable`, und Arrays und Dictionaries genügen ebenfalls `Codable`, wens sie `Codable`-Objekte enthalten. Das heißt, `[String]` genügt `Codable` problemlos, da `String` selbst `Codable` genügt. 

Hier benötigen wir allerdings etwas Komplexeres: jede Petition enthält einen Titel, einen Textkörper, die Anzahl der Unterschriften, und mehr. Das heißt, wir müssen unseren eigenen maßgeschneiderten Datentyp definieren anstatt einfach nur  `String` oder `Int` zu verwenden.

Swift hat zwei Wege, um eigene Datentypen zu erzeugen, die als Structs und Klassen bekannt sind. Wir haben Klassen schon verwendet - alle unsere `UIViewController`-Subklassen sind Klassen - aber wenn du mit eigenen Daten arbeitest anstatt eine Subklasse zu erzeugen, ist es im Allgemeinen besser, mit Structs zu arbeiten, da sie einfacher sind. 

Wir werden ein eigenes Struct `Petition` erzeugen, das eine Petition aus unserer JSON enthält, das heißt, es wird den Titel-String, den Textkörper-String, und die Anzahl der Unterschriften, eine Ganzzahl, speichern. Drücke daher Cmd+N und wähle eine neue Swift-Datei mit dem Namen Petition.swift.

    struct Petition {
        var title: String
        var body: String
        var signatureCount: Int
    }

Dies definiert ein spezielles Struct mit drei Eigenschaften. Einer der Vorteile von Structs in Swift ist, dass sie uns einen *elementweisen Initialisierer* geben - eine spezielle Funktion, die neue `Petition`-Instanzen erzeugen kann, indem Werte für `title`, `body` und `signatureCount` übergeben werden.

Dazu kommen wir gleich, aber zunächst hatte ich das `Codable`-Protokoll erwähnt. Unser `Petition` Struct enthält zwei Strings und eine Ganzzahl, die alle bereits `Codable` genügen, daher können wir Swift sagen, dass der ganze `Petition`-Typ `Codable` erfüllen soll, und zwar so: 

    struct Petition: Codable {
        var title: String
        var body: String
        var signatureCount: Int
    }

Mit dieser simplen Änderung sind wir beinahe soweit, Instanzen von `Petition` aus JSON zu laden.

Ich sage *beinahe* soweit, denn da ist noch ein kleiner Haken in unserem Plan: wenn du dir das oben gegebene JSON-Beispiel angeschaut hast, ist dir aufgefallen, dass unserer Petitions-Array tatsächlich innerhalb eines Dictionaries namens "results" liegt. Das bedeutet, wenn wir versuchen, Swift den JSON-Code parsen zu lassen, müssen wir zuerst den Schlüssel laden, und dann *darin* das Array der Petitions-Resulate laden. 

Swifts `Codable`-Protokoll muss exakt wissen, wo es die Daten findet, was in diesem Falle bedeutet, ein *zweites* Struct zu machen. Dieses wird eine einzelne Eigenschaft namens `results` haben, welches ein Array von unseren `Petition`-Structs ist. Das passt dann exakt dazu, wie JSON aussieht: die Haupt-JSON enthält das `results`-Array, und in jedem Element in diesem Array ist eine `Petition`.

Drücke also erneut Cmd+N, um eine neue Datei zu erzeugen, wähle Swift-Datei und nenne sie Petitions.swift. Gib ihr diesen Inhalt: 

    struct Petitions: Codable {
        var results: [Petition]
    }

Ich sehe ein, dass das nach einer Menge Arbeit aussieht, aber vertrau mir: es wird noch viel leichter!

Alles, was wir getan haben, war, die Datenstrukturen zu definieren, in die wir die JSON laden wollen. Der nächste Schritt ist, eine Eigenschaft in `ViewController` zu erzeugen, die unser Petitions-Array speichert.

Wie du dich erinnerst, deklarierst du Arrays, indem du einfach den Datentyp in eckigen Klammern schreibst, so wie hier:

    var petitions = [String]()

Wir wollen ein Array aus unserem `Petition`-Objekt machen. Das sieht dann so aus:

    var petitions = [Petition]()

Ersetze damit die aktuelle `petitions`-Definition am Anfang von ViewController.swift.

Jetzt ist es an der Zeit, JSON zu parsen, was bedeutet, sie zu verarbeiten und die Inhalte zu prüfen. Wir fangen damit an, die `viewDidLoad()`-Methode für `ViewController` so anzupassen, dass sie die Daten vom Petitions-Server des Weißen Hauses herunterlädt, sie in ein Swift `Data`-Objekt umwandelt, und dann versucht, dieses in ein Array von `Petition`-Instanzen zu konvertieren.

Wir haben `Data` bisher noch nicht verwendet. Wie `String` und `Int` ist dies einer der fundamentalen Datentypen von Swift, auch wenn er auf noch niedrigerer Stufe angesiedelt ist - er enthält buchstäblich irgendwelche Binärdaten. Das könnte ein String sein, das könnte der Inhalt einer Zip-Datei sein, or buchstäblich irgendetwas anderes.

`Data` und `String` haben einige Dinge gemeinsam.
Du hast bereits gesehen, dass ein `String` mit Hilfe von `contentsOfFile` erzeugt werden kann, um Daten von der Festplatte zu laden, und `Data` hat genau den gleichen Initialisierer. Beide können also mit einem `contentsOf`-Initialisierer erzeugt werden, der Daten von einer URL (mit `URL` spezifiziert) herunterlädt und dir zur Verfügung stellt.

Das ist für unsere Zwecke perfekt - hier ist die neue `viewDidLoad`-Methode:

    override func viewDidLoad() {
        super.viewDidLoad()

        let urlString = "https://api.whitehouse.gov/v1/petitions.json?limit=100"

        if let url = URL(string: urlString) {
            if let data = try? Data(contentsOf: url) {
                // we're OK to parse!
            }
        }
    }

Let's focus on the new stuff:

- `urlString` points to the Whitehouse.gov server, accessing the petitions system.
- We use `if let` to make sure the `URL` is valid, rather than force unwrapping it. Later on you can return to this to add more URLs, so it's good play it safe.
- We create a new `Data` object using its `contentsOf` method. This returns the content from a `URL`, but it might throw an error (i.e., if the internet connection was down) so we need to use `try?`.
- If the `Data` object was created successfully, we reach the “we're OK to parse!” line. This starts with `//`, which begins a comment line in Swift. Comment lines are ignored by the compiler; we write them as notes to ourselves.

This code isn't perfect, in fact far from it. In fact, by downloading data from the internet in `viewDidLoad()` our app will lock up until all the data has been transferred. There are solutions to this, but to avoid complexity they won't be covered until project 9.

For now, we want to focus on our JSON parsing. We already have a `petitions` array that is ready to accept an array of petitions. We want to use Swift’s `Codable` system to parse our JSON into that array, and once that's done tell our table view to reload itself.

Are you ready? Because this code is remarkably simple given how much work it's doing:

    func parse(json: Data) {
        let decoder = JSONDecoder()

        if let jsonPetitions = try? decoder.decode(Petitions.self, from: json) {
            petitions = jsonPetitions.results
            tableView.reloadData()
        }
    }

Place that method just underneath `viewDidLoad()` method, then replace the existing `// we're OK to parse!` line in `viewDidLoad()` with this:

    parse(json: data)

This new `parse()` method does a few new and interesting things:

1. It creates an instance of `JSONDecoder`, which is dedicated to converting between JSON and `Codable` objects.
2. It then calls the `decode()` method on that decoder, asking it to convert our `json` data into a `Petitions` object. This is a throwing call, so we use `try?` to check whether it worked.
3. If the JSON was converted successfully, assign the `results` array to our `petitions` property then reload the table view.

The one part you haven’t seen before is `Petitions.self`, which is Swift’s way of referring to the `Petitions` type itself rather than an instance of it. That is, we’re not saying “create a new one”, but instead specifying it as a parameter to the decoding so `JSONDecoder` knows what to convert the JSON too.

You can run the program now, although it just shows “Title goes here” and “Subtitle goes here” again and again, because our `cellForRowAt` method just inserts dummy data.

We want to modify this so that the cells print out the `title` value of our `Petition` object, but we also want to use the subtitle text label that got added when we changed the cell type from "Basic" to "Subtitle" in the storyboard. To do that, change the `cellForRowAt` method to this:

    let petition = petitions[indexPath.row]
    cell.textLabel?.text = petition.title
    cell.detailTextLabel?.text = petition.body

We set the `title`, `body` and `sigs` keys in object, so now we can read them out to configure our cell correctly.

If you run the app now, you'll see things are starting to come together quite nicely – every table row now shows the petition title, and beneath it shows the first few words of the petition's body. The subtitle automatically shows "…" at the end when there isn't enough room for all the text, but it's enough to give the user a flavor of what's going on.
