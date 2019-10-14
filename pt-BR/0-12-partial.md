# Opcionais

<!-- YOUTUBE: OkzZ3T3lrlg -->

Swift é uma linguagem muito segura, com isso quero dizer que ela tenta garantir que seu código nunca falhe de jeitos surpreendentes.

Um dos modos mais comuns de um código falhar é quando ele tenta usar um dado de tipo diferente ou que não pode ser encontrado. Por exemplo, imagine uma função assim:

    func getHaterStatus() -> String {
        return "Hate"
    }

A função não aceita nenhum parâmetro e retorna a cadeia de caracteres: "Hate". Mas se hoje é um dia ensolarado e os *haters* não. Mas e se hoje for um dia particularmente ensolarado, e esses *haters* não sentirem ódio - e então? Bem, talvez não queremos devolver nada: esse *hater* não está odiando hoje.

Agora, quando se trata de uma sequência, você pode pensar que uma sequência vazia é uma ótima maneira de comunicar nada, e isso pode ser verdade às vezes. Mas e os números - 0 é um "número vazio"? Ou -1?

Antes de começar a tentar criar regras imaginárias, o Swift tem uma solução: opcionais. Um valor opcional é aquele que pode ter um valor ou não. A maioria das pessoas acha os opcionais difíceis de entender, e tudo bem - vou tentar explicar de várias maneiras, e espero que alguma faça sentido pra você.

Por enquanto, imagine uma pesquisa em que você pergunte a alguém: “Em uma escala de 1 a 5, quão impressionante é Taylor Swift?” - o que alguém responderia se nunca tivesse ouvido falar dela? 1 seria repreende-la injustamente e 5 estaria elogiando quando esse pessoa não faz idéia quem é Taylor Swift. A solução é opcional: "Não quero fornecer um número".

Quando usamos `-> String` significa que "essa função definitivamente vai retornar uma cadeia de caracteres", o que significa que ela **não** pode retornar nenhum valor, e por isso pode ser chamada com conhecimento garantido que você sempre poderá usar seu resultado como uma cadeia de caracteres. Se quisermos indicar ao Swift que essa função pode retornar um valor ou não, precisamos usar a simbologia abaixo:

    func getHaterStatus() -> String? {
        return "Hate"
    }

Note o sinal de interrogação adicional: `String?` means “optional string.” Agora, em nosso caso, ainda estamos retornando “Hate” não importa a situação, mas vamos modificar a função para que: se hoje é um dia ensolarado, os *haters* viraram a página e desistiram do seu ódio, então queremos não retornar valor. Em Swift, este "sem valor" tem um nome especial: `nil`.

Mude a função para:

    func getHaterStatus(weather: String) -> String? {
        if weather == "sunny" {
            return nil
        } else {
            return "Hate"
        }
    }

Ela aceira um parâmetro de cadeia de caracteres (o tempo) e retorna uma cadeia de caracteres (o ódio), mas esse valor pode existir ou não – é *nil*. Neste caso, significa que podemos receber no retorno uma cadeia de caracteres,, ou *nil*.

Agora a parte importante: Swift quer que seu código seja seguro, e tentar usar um valor *nil* é uma péssima ideia. Pode quebrar seu código, pode estragar sua lógica, ou pode fazer a interface de usuário mostrar algo errado. Como resultado, quando você declara um valor sendo opcional, Swift irá fazer você tratar este valor de forma segura.

Vamos tentar isso agora: adicione estas linhas de código no *playground*:

    var status: String
    status = getHaterStatus(weather: "rainy")

A primeira linha cria uma variável, e a segunda atribui o valor de `getHaterStatus()` – e hoje o dia está chuvoso, então *haters* estão espalhando seu ódio com certeza.

Este código não irá rodar, porque dissemos que `status` é do tipo `String`, o que requer um valor, mas `getHaterStatus()` pode não retornar um pois seu resultado de saída é uma cadeia de caracteres opcional. Isto é, dissemos que **definitivamente** haveria uma cadeia de caracteres em `status`, mas `getHaterStatus()` pode não retornar nada. Swift não irá deixar você cometer este erro, o que é extremamente útil porque efetivamente evita um monte de error muito comuns.

Para corrigir o problema, precisamos transformar a variável `status` em `String?`, ou retiramos a anotação de tipo e deixamos o Swift usar sua inferência de tipos. A primeira opção seria assim:

    var status: String?
    status = getHaterStatus(weather: "rainy")

E a segunda:

    var status = getHaterStatus(weather: "rainy")

Independente de qual você escolher, o valor poderá estar lá ou não, e por padrão Swift não deixará você usar o valor de forma leviana. Como exemplo, imagine uma função assim:

    func takeHaterAction(status: String) {
        if status == "Hate" {
            print("Hating")
        }
    }

Ela recebe uma cadeia de caracteres e imprime uma mensagem dependendo do seu conteúdo. Essa função recebe uma `String` , e **não** uma `String?` – você não pode passar um opcional aqui, ela precisa realmente de uma cadeia de caracteres, o que significa que não podemos chamá-la com a variável `status`.

Swift tem duas soluções. As duas são usadas, mas uma é preferível sobre a outra. A primeira é chamada desempacotamento de opcional, e é feita dentro de uma declaração condicional usando sintaxe especial. Ela faz duas coisas ao mesmo tempo: verifica se uma opcional tem um valor, e caso tenha desempacota em um tipo não opcional e executa o bloco de código.

A sintaxe parece com esta:

    if let unwrappedStatus = status {
        // unwrappedStatus contains a non-optional value!
    } else {
        // in case you want an else block, here you go…
    }

Essa declaração `if let` verifica e desempacota em uma linha de código sucinta, o que a torna muito comum. Usando este método, nós podemos desempacotar seguramente o retorno de `getHaterStatus()` e ter certeza que só chamaremos a função `takeHaterAction()` com uma cadeia de caracteres válida e não opcional. Veja o código completo:

    func getHaterStatus(weather: String) -> String? {
        if weather == "sunny" {
            return nil
        } else {
            return "Hate"
        }
    }

    func takeHaterAction(status: String) {
        if status == "Hate" {
            print("Hating")
        }
    }

    if let haterStatus = getHaterStatus(weather: "rainy") {
        takeHaterAction(status: haterStatus)
    }

**Se você entendeu este conceito, fique livre para pular até o título que diz "Desempacotamento forçado de opcionais".** Se ainda não tem muita clareza sobre opcionais, continue lendo.

OK, se você ainda está aqui significa que a explicação acima não fez sentido pra você, ou você meio que entendeu mas gostaria de mais explicação. Opcionais são usadas extensivamente em Swift, então você realmente precisa entende-los. Vou tentar explicar novamente, de outra maneira, e espero que ajude!

Aqui está a nova função:

    func yearAlbumReleased(name: String) -> Int {
        if name == "Taylor Swift" { return 2006 }
        if name == "Fearless" { return 2008 }
        if name == "Speak Now" { return 2010 }
        if name == "Red" { return 2012 }
        if name == "1989" { return 2014 }

        return 0
    }

Ela recebe o nome de um álbum da Taylor Swift, e retorna o ano em que ele foi lançado. Mas,se chamarmos a função com o nome de álbum "Lantern", porque confundimos Taylor Swift com Hudson Mohawke (um erro fácil de cometer, certo?) então ela iré retornar 0 porque não é um álbum da Taylor Swift.

Mas, faz sentido retornar 0? Claro, se o álbum tivesse sido lançado em 0 AD quando César Augusto era o imperador de Roma, 0 faria sentido, mas aqui só confunde – precisamos saber antecipadamente que 0 significa "não reconhecido".

Uma ideia muito melhor é reescrever essa função para que ela retorne um número inteiro (quando um álbum foi encontrado) ou *nil* (quando nada foi encontrado), o que é muito mais fácil graças aos opcionais. Aqui está a função:

    func yearAlbumReleased(name: String) -> Int? {
        if name == "Taylor Swift" { return 2006 }
        if name == "Fearless" { return 2008 }
        if name == "Speak Now" { return 2010 }
        if name == "Red" { return 2012 }
        if name == "1989" { return 2014 }

        return nil
    }

Agora que retorna *nil*, precisamos desempacotar o resultado usando `if let` porque precisamos checar se o valor existe ou não.

**Se você entendeu este conceito, fique livre para pular até o título que diz "Desempacotamento forçado de opcionais".** Se ainda não tem muita clareza sobre opcionais, continue lendo.

OK, se você ainda está aqui significa que o conceito de opcionais está bem confuso você, então vou tentar explicá-los uma última vez.

Aqui está uma matriz de nomes:

    var items = ["James", "John", "Sally"]

Se quiséssemos escrever uma função que busque essa matriz e diga o índice de um nome, escreveríamos algo como::

    func position(of string: String, in array: [String]) -> Int {
        for i in 0 ..< array.count {
            if array[i] == string {
                return i
            }
        }

        return 0
    }

Ela itera sobre todos os itens da matriz, retornando a posição se encontrar um correspondente, caso contrário retorna 0.

Agora tente rodar essas quatro linhas de código:

    let jamesPosition = position(of: "James", in: items)
    let johnPosition = position(of: "John", in: items)
    let sallyPosition = position(of: "Sally", in: items)
    let bobPosition = position(of: "Bob", in: items)

A resposta será 0, 1, 2, 0 – as posições de James e Bob são a mesma, mesmo que um exista na mariz e outro não. Isso ocorre porque usei 0 para significar "não encontrado". Uma correção fácil seria fazer -1 representar não encontrado, mas 0 ou -1 você ainda precisa lembrar que esse valor específico significa "não encontrado".

A solução são os opcionais: retorne um número inteiro se encontrou um valor ou *nil* caso contrário. De fato, essa é a abordagem que o método interno "buscar na matriz" usa: `someArray.firstIndex(of: someValue)`.

Quando trabalhamos com esses valores que "podem existir ou não", Swift força você a desempacota-los antes de usar, deste modo thus reconhecendo que pode não existir um valor. Isso é o que essa sintaxe `if let` faz: se o opcional tem valor desempacote e use, caso contrário não use. Você não terá como receber um valor possívelmente vazio, porque Swift não vai deixar.

Se você **ainda** não tem certeza de como os opcionais funcionam, então é melhor me contatar no Twitter e eu tentarei ajudar: você pode me encontrar como [@twostraws](http://twitter.com/twostraws).


## Desempacotamento forçado de opcionais

Swift deixa você sobrepor sua segurança se usar a ponto de exclamação: `!`. Se você sabe que um opcional vai definitivamente ter um valor, você pode forçar o desempacotamento colocando o ponto de exclamação depois do opcional. 

**Mas por favor tenha cuidado: se você tentar usso numa variável que não tem valor, seu código irá quebrar.**

Para mostrar um exemplo funcional, aqui está um código base:

    func yearAlbumReleased(name: String) -> Int? {
        if name == "Taylor Swift" { return 2006 }
        if name == "Fearless" { return 2008 }
        if name == "Speak Now" { return 2010 }
        if name == "Red" { return 2012 }
        if name == "1989" { return 2014 }

        return nil
    }

    var year = yearAlbumReleased(name: "Red")

    if year == nil {
        print("There was an error")
    } else {
        print("It was released in \(year)")
    }

Esse pedaço de código That gets the year an album was released. If the album couldn't be found, `year` will be set to nil, and an error message will be printed. Otherwise, the year will be printed.

Or will it? Well, `yearAlbumReleased()` returns an optional integer, and this code doesn't use `if let` to unwrap that optional. As a result, it will print out the following: "It was released in Optional(2012)" – probably not what we wanted!

At this point in the code, we have already checked that we have a valid value, so it's a bit pointless to have another `if let` in there to safely unwrap the optional. So, Swift provides a solution – change the second `print()` call to this:

    print("It was released in \(year!)")

Note the exclamation mark: it means "I'm certain this contains a value, so force unwrap it now."


## Implicitly unwrapped optionals

You can also use this exclamation mark syntax to create implicitly unwrapped optionals, which is where some people really start to get confused. So, please read this carefully!

- A regular variable must contain a value. Example: `String` must contain a string, even if that string is empty, i.e. `""`. It *cannot* be nil.
- An *optional* variable might contain a value, or might not. It must be unwrapped before it is used. Example: `String?` might contain a string, or it might contain nil. The only way to find out is to unwrap it.
- An implicitly unwrapped optional might contain a value, or might not. But it does *not* need to be unwrapped before it is used. Swift won't check for you, so you need to be extra careful. Example: `String!` might contain a string, or it might contain nil – and it's down to you to use it appropriately. It’s like a regular optional, but Swift lets you access the value directly without the unwrapping safety. If you try to do it, it means you know there’s a value there – but if you’re wrong your app will crash.

The main times you're going to meet implicitly unwrapped optionals is when you're working with user interface elements in UIKit on iOS or AppKit on macOS. These need to be declared up front, but you can't use them until they have been created – and Apple likes to create user interface elements at the last possible moment to avoid any unnecessary work. Having to continually unwrap values you definitely know will be there is annoying, so these are made implicitly unwrapped.

Don't worry if you find implicitly unwrapped optionals a bit hard to grasp - it will become clear as you work with the language.
