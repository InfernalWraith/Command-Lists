# Go
## Basics
Official docs can be found at https://pkg.go.dev/std

A Go package can be thought of as a project or a workspace. It's a collection of common source code files. A package can have many related files inside of it, with each file ending in the `.go` file extension. Each file in the package needs to have the statement `package PACKAGE_NAME` at the top (so `package examplePackage` for a package called `examplePackage`).

There are two types of packages: **executable** and **reusable**. **Executable** packages create an executable file once built. Reusable packages are like code dependencies or libraries, they contain reusable logic and helper functions which help us reuse code on future projects. The name of the package determines if a package is executable or reusable - a package is named `main` to make it an executable package, while any other name results in reusable packages. An executable package **must** have a function inside it called `main`, which runs automatically once the program is executed.

Packages can be imported using the `import "examplePackage"` command. When importing multiple packages, parentheses should be opened after the `import` keyword and each package should go on a new line:

```go    
import (
    "fmt"
    "os"
    "strings"
)
```


### Console commands
Here are some essential console commands when working with Go:
- `go` - Lists available commands
- `go build` - Compiles files
- `go run` - Compiles and executes files
- `go fmt` - Formats all the code in each file in the current directory
- `go install` - Compiles and "installs" a package
- `go get` - Downloads the raw source code of someone else's package
- `go test` - Runs any tests associated with the current project

## Variables and functions
Variables in Go are static types. The type can be explicitly defined, or it can be automatically inferred from the value assigned during initialization. Another way to initialize a variable is by using the `:=` operator, which is somewhat shorter than the alternatives. The `:=` operator can only be used when a variable is initialized, it can't be used later on to assign a new value to it (the standard `=` is used then).

```go    
// All these ways to initialize are equivalent
var text string = "This is some text"
var text = "This is some text"
text := "This is some text"
```


Type conversion in Go is done like so:

```go    
text := string(65)
fmt.Println(text) // Prints out 'A', since A is 65 in ASCII
```


Functions are declared using the `func` keyword, followed by standard parentheses in which a list of arguments is specified, after which the curly braces which contain the function body are placed. Return types, if any, should be placed between the parentheses and the curly braces. The content which is returned from the function must match the type defined in the function prototype. If returning multiple things, another set of parentheses containing the types of all the returned variables should be between the argument parentheses and the curly braces.

```go    
func exampleFunction() {
    // This function doesn't return anything
}

//          Arguments   |     |   Return type
//                     \/    \/
func thisReturnsAString() string {
    return "This is a string"
}

func thisReturnsTwoStringsAndAnInt() (string, string, int) {
    return "This is a string", "String 2", 15
}
```


Variables can be declared outside of a function, but values can't be assigned to them there.

```go    
var exampleText string

func main() {
    exampleText = "test"
    fmt.Println(exampleText)
}
```


Function literals are analogous to lambda functions in **C#** or anonymous functions in **JS**.

```go
func main() {
    func() {
        fmt.Println("Starting!")
        time.Sleep(3 * time.Second)
        fmt.Println("I waited for 3 seconds!")
    }()
//   /\    Calling the function right away, not even assigning it to a variable


    // Assigning the function literal to a variable
    sayHi := func() {
        fmt.Println("Hi there!")
    }
    
    sayHi()
}
```

___

## Slices and loops
In Go, an **array** is a fixed length list of things, whereas a **slice** is an array that can grow or shrink. They must be defined with a data type, and every element inside of an array or a slice must be of the same type. Declaring a slice in Go can be done like so:

```go    
// mySlice := []TYPE{ELEMENTS}
myText := []string{"Text 1", textReturnFunction(), "Text 3"}
// 'append' doesn't mutate the original, but returns a new slice
myText = append(myText, "Text 4")

// Declare a byte slice that has space for 10000 elements
bs := make([]byte, 10000)
```


Iterating over a list of items can be done using `for` loops.

```go
// i - Counter/Index
// card - Name which will be used to reference each element
// cards - Collection of elements whose elements we're iterating over
for i, card := range cards {
    fmt.Println(i, card)
}
```


All variables declared in Go must be used, but whenever there's a variable we don't want to use (such as the index in the `for` loop, we can replace it with an underscore `_`).

```go    
// We don't need the index, so we put a '_' where it should be
for _, card := range cards {
    fmt.Println(card)
}

// Does the same thing
for i:=0; i < len(cards); i++ {
    fmt.Println(cards[i])
}
```


Slices are accessed using the `[]` operator, like in a lot of other languages (`mySlice[0]` for the 1st element, `mySlice[5]` for the 6th, etc). Like in Python, a part of the slice can be selected by using the `[start_index:end_index]` operator, where the start is included but the end index isn't. If we want the selection to go from the beginning to the end, we can simply leave out the first or second argument of `[:]`, respectively. The length of a slice can be returned by using `len(mySlice)`.

```go    
//    Index          0        1         2       3
items := []string{"First", "Second", "Third", "Fourth"}

// Return a slice containing "First" and "Second"
items[0:2]
items[:2]

// Return a slice containing "Second", "Third" and "Fourth"
items[1:4]
items[1:]
items[1:len(items)]
```


Elements in slices can quickly be swapped like so:

```go    
items[x], items[y] = items[y], items[x]
```

___

## Custom types
Go isn't an object oriented programming language, so there is no concept of classes in it. However, we can still make new types as specialized version of existing types - essentially adding custom functionality to existing types.

```go    
// Package, imports, etc.

// 'deck' will have the same properties as a string slice
// Now instead of cards := []string{"Card 1", "Card2"}
// We can do      cards := deck{"Card 1", "Card2"}
type deck []string
```


Receiver functions are similar to methods in object oriented programming languages, in the sense that they're called on an object of a certain type (`variable.receiverFunction()`). They differ from regular functions by having parentheses between the `func` keyword and the function name, in which an argument which defines what the type of the variable it's called on will be (so `func (d deck) someFunc()` will be called on a `deck` type variable). The convention with receiver function arguments is for them to be 1-3 letters short, and to be an abbreviation of the actual thing they're supposed to be.

```go    
// Adds a receiver function called 'print' to all 'deck' variables
// Invoked like so: cards.print()
// 'd' is the reference to the variable the function is called on (cards)
func (d deck) print() {
    for i, card := range d {
        fmt.Println(i, card)
    }
}

// Not a receiver, called like a standard function, not like a method
func newDeck() deck {
    cards := deck{}
    return cards
}
```


If we're using a receiver function that doesn't make use of the variable it's called on, then we can just omit the variable's name in the function prototype.

```go    
//    \/  Has a variable 'd' that's never used
func (d deck) getInfo() {
    return "This is a standard deck of 52 playing cards."
}

// No variable, equivalent to the other approach
func (deck) getInfo() {
    return "This is a standard deck of 52 playing cards."
}
```

___

## Testing
To make a test, create a new file ending in `_test.go` (like `deck_test.go`). Tests must also specify which package they belong to. When we want to test a function, we should create a function inside the test file with the same name as the function we want to test, but with the word **Test** as a prefix (so for a function `myFun`, the test would be `TestMyFun`). If testing multiple functions at once, we should include their names in the test function name (for functions `myFun` and `sayHello`, it would be `TestMyFun_SayHello`).

```go    
// deck.go
    type deck []string
    
    // Creates a deck - A string slice with the standard 52 playing cards
    func newDeck() deck {
    	cards := deck{}
    	cardSuits := []string{"Spades", "Diamonds", "Hearts", "Clubs"}
    	cardValues := []string{"Ace", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Jack", "Queen", "King"}
    
        for _, suit := range cardSuits {
            for _, value := range cardValues {
                cards = append(cards, value+" of "+suit)
            }
        }
    
    	return cards
    }

// deck_test.go
    package main

    import "testing"
    
    func TestNewDeck(t *testing.T) {
    	d := newDeck()
    
    	if len(d) != 52 {
    	    t.Error("Expected deck length of 52, but got", len(d))
    	}
    
    	if d[0] != "Ace of Spades" {
    	    t.Error("Expected first card to be 'Ace of Spades' but got", d[0])
    	}
    }
```


After tests are created, they can be run with `go test`. Keep in mind that Go doesn't clean up any files created by testing (for example when testing a function which writes data to a file), so that has to be done manually inside the test function.

___

## Structs
A `struct` in Go is a collection of properties that have a common purpose (such as defining a complex object). Structs are similar to those in *C*, objects in *JS* or dictionaries in *Python*. They're created like so:

```go    
type person struct {
    firstName string
    lastName  string
}
```

Once a `struct` is created, a variable of that type can be created by either passing in values in the order in which they're listed in the definition of the `struct`, or by explicitly stating `struct` properties and assigning values to them.

```go    
// All three approaches work
john := person{"John", "Doe"}
jane := person{firstName: "Jane", lastName: "Doe"}
var mike person
mike.firstName = "Mike"
mike.lastName = "Doe"
```

New structs can be printed by default, and an overview of their contents can be viewed using the `fmt.Printf("%+v", myStruct)` command. The `%+v` formatting parameter will print out all properties and their values.

```go    
// Prints out "{John Doe}"
fmt.Println(john)
```

Structs can also be used as properties in other structs, and receiver functions for structs can be created.

```go    
type contactInfo struct {
    email   string
    zipCode int
}

type person struct {
    firstName string
    lastName  string
    contact   contactInfo
}
    // Note: instead of 'contact contactInfo', we can write just
    // 'contactInfo', which is the same as writing out
    // 'contactInfo contactInfo' - same attribute name as the struct

func (p person) print() {
    fmt.Printf("%+v", p)
}

jim := person {
    firstName: "Jim",
    lastName: "Doe",
    contact: contactInfo {
        email: "jimdoe@gmail.com",
        zipCode: 12345,
//    |    Look!     /\  ALL multiline struct declarations 
//   \/              |   MUST have a comma at the end of the line
    },
}

jim.print()
```


Go passes by value, not by reference, so when passing a variable to a function, all changes are made on a copy of it made specifically for usage inside the function. To be able to change the value we're calling the receiver function on, we need to pass an address to it, which is obtained using the `&` unary operator (for a variable `jim`, the address is obtained by using `&jim`). Note that if we're calling functions in the same line where we apply the `&` operator (like `&jim.someFunc()`), we need to surround the operator and the variable it's called on in parentheses (so `(&jim).someFunc()`), otherwise the `&` is applied to the result of the function. After we've passed the pointer using `&`, we need to dereference the pointer using the `*` operator (like `(*jim).firstName = "jim"`). When the `*` symbol is in front of a **type**, however, then it isn't dereferencing it, but rather saying that the type is a pointer to that type (`*person` says the type is a **pointer** *to a variable of the type* **person**). In short: 

* We turn **address** into **value** with **\*address**
* We turn **value** into **address** with **&value**

Essentially, everything behaves just like in **C** or **C++**.

```go    
func (p person) updateName (fn string) {
    p.firstName = fn
}

//      \/     Pointer to a person!
func (p *person) updateNameWithPointer (fn string) {
//   \/     Dereferencing the pointer into a value
    (*p).firstName = fn
}

func main() {
    // Using the same values for jim as the earlier example
    jim.updateName("Jimmy")
    jim.print() // Prints out Jim instead of Jimmy
    
    (&jim).updateNameWithPointer("Jimbo")
    jim.print() // Prints out Jimbo
    
    // This also works, implicit conversion from 'person' to '*person'
    jim.updateNameWithPointer("Jimtastic")
}
```

A `slice` is essentially a reference to an array, as well as a structure that holds extra attributes like its length, capacity and a pointer to the actual array that stores the data. Thus, if a slice is changed inside of a function, even without a pointer, it also changes outside of the function. This is because only the aforementioned extra attributes are copied over to make the local `slice` variable for that function, whereas the main array isn't.

```go    
func updateSlice(s []string) {
    s[0] = "Bye"
}

func main() {
    mySlice := []string{"Hi", "Mark"}
    updateSlice(mySlice)
    fmt.Println(mySlice) // Prints 'Bye Mark'
}
```

This doesn't only apply to `slice` variables, but also to other types. These **reference types** are `slice`, `map`, `channel`, `pointer`, `function`, whereas **value types** are `int`, `float`, `string`, `bool`, `struct`. Value types need pointers to be changed in a function, whereas reference types don't.

___

## Maps
Maps, like dictionaries in other languages, are collections **key-value pairs**. Both the keys and the values are statically typed - the keys and values can be different types, but each key must be of the same type as other keys, and each value in the map must also be of the same type as other values. Elements can be removed using the `delete` function.

```go    
numbers := map[int]string{
    1: "One",
    2: "Two"
}

// Both of these lines do the same thing
var numbers map[int]string
numbers := make(map[string]string)

numbers[3] = "Three"
delete(numbers, 2)  // Now there are only 1:"One" and 3:"Three" in the map
```


Keys in maps are indexed, so we can iterate through them.

```go    
for key, value := range exampleMap {
    fmt.Println("Value for", key, "is", value)
}
```

___

## Interfaces
Unlike in **C** or **C++**, Go doesn't support functions which have the same name but different arguments, instead showing an error that the function is redeclared. In order to avoid code duplication when using the same function code for different arguments, we should create an `interface`. 

```go    
// If you are a type with a function 'getGreeting' that returns a string
// Then you may be considered a type 'bot' when calling functions
type bot interface {
    getGreeting() string
}

type englishBot struct{}
type spanishBot struct{}

func (englishBot) getGreeting() string { return "Hello" }
func (spanishBot) getGreeting() string { return "Hola" }

// Accepts 'bot'      \/
func printGreeting(b bot) {
    fmt.Println(b.getGreeting())
}

func main() {
    eb := englishBot{}
    sb := spanishBot{}

    printGreeting(eb) // Prints 'Hello'
    printGreeting(sb) // Prints 'Hola'
}
```


Go doesn't have generic types, and interfaces aren't generic types. Interfaces are implicit, since we don't have to explicitly state that a certain type implements an interface. Interfaces are essentially a contract to help us manage types. Multiple interfaces can be embedded into one to create an interface which requires a type to satisfy every interface listed in it.

```go    
// Must satisfy both SomeInterface and OtherInterface to be CombinedInterface
type CombinedInterface interface {
    SomeInterface
    OtherInterface
}
```


The `Reader` interface is commonly used in Go to satisfy the requirement of reading different data types (mainly from unknown outside data sources) and converting them into a byte slice, so that different functions for each data type don't have to be implemented. This is done using the required `Read([]byte)(int, error)` function, which takes data and puts it into a `[]byte` variable provided as an argument. The `Read` function returns the number of bytes which were read into the slice (in the returned `int`) and an error, if one occurred (in the returned `error`). Because the `Read` function reads data into the byte slice until it is full, and because it doesn't implement the option to resize a byte slice, a byte slice with enough space to hold the data should be passed (such as one created with `make`).
    
```go
resp, err := http.Get("http://google.com")

if err != nil {
    fmt.Println("Error in HTTP GET")
    os.Exit(1)
}

// Allocate enough space for the 'Read' function
bs := make([]byte, 99999)
resp.Body.Read(bs)
fmt.Println(string(bs))
```


The `Writer` interface requires the implementation of a `Write([]byte)(int, error)` function, which essentially does the opposite of the `Read` function - it takes a byte slice and uses the data inside to produce an output.

___

## Routines
By default, the Go scheduler uses only one CPU core. When using one core, routines are executed concurrently (if one thread blocks, another is picked up and worked on), while using multiple cores allows for parallelism (executing multiple routines at the same time). A routine can be created using the `go` keyword in front of a function call (`go exampleFunction()` creates a routine which runs `exampleFunction()`). The only way to communicate between Go routines is by using **channels**. 

A channel intermediates communication between different routines - sending a message to a channel relays that message to any routine that has access to that channel. Just like any other variables, channels are typed, in the sense that the data passed into them must be of the same type. Thus, a channel of type `string` only accepts messages of type `string`, a channel of type `float` only accepts `float` values as messages etc. A channel can be created by using the `make` function. Receiving messages from a channel is a blocking line of code - program execution waits for a message to come through before a routine will continue executing.

```go
func main() {
    // Create a channel that takes string messages
    c := make(chan string)
    
    // Send some strings into the channel
    c <- "Hello"
    c <- "Hola"
    c <- "Hallo"
    
    for i:=0; i<3; i++ {
        // Create a routine which runs 'exampleFunction'
        go exampleFunction(i, c)
    }
}

// Pass the channel to the function so it knows what to communicate with
func exampleFunction(someArgument int, ch chan string) {
    // Wait for a value from the channel and assign it to 'channelMessage'
    channelMessage <- ch
    fmt.Println(channelMessage)
    
    // The same as above can be done without assigning to a variable
    // fmt.Println(<-ch)
}
```


It's possible to infinitely repeat a routine by passing a message into the channel containing the arguments needed to run the routine function again.

```go
// Checks the status of several websites
func main() {
    links := []string{
    	"http://google.com",
    	"http://youtube.com",
    	"http://stackoverflow.com",
    }
    
    c := make(chan string)
    
    // Start a routine for all the links
    for _, link := range links {
    	go checkLink(link, c)
    }
    
    // Repeat every routine as soon as it finishes
    for {
    	go checkLink(<-c, c)
    }
    
    // This approach does the same thing as the one above, just more legible
    for l := range c {
        go checkLink(l, c)   
    }
}

func checkLink(link string, c chan string) {
    _, err := http.Get(link)
    if err != nil {
    	fmt.Println(link, "might be down!")
    	c <- link
    	return
    }
    
    fmt.Println(link, "is up!")
    c <- link
}
```


Routines can be paused with `time.Sleep` in cases where running them too often wouldn't be desirable (such as spamming online requests). Pausing routines should be done inside helper routines (such as ones created using function literals) as to not have the functions pause if used outside of routines (if we want them to pause somewhere, best to make them pause **only** there).

```go
for l := range c {
    // Pause the routine inside a helper function literal routine
    go func() {
        someFunction(3, c)
        time.Sleep(5 * time.Second)
    }()
//   /\   Need to call the function when making it a routine function
}
```


Referencing the same variable inside both the main routine and a child routine is a very bad idea, because the variable might be changed inside one of the routines without the other knowing that. Instead of doing that, passing by value as one of the child routine function arguments (and thus making a copy for that routine) should be done.

```go
for n := range c {
    // This might not work properly, since 'num' might change
    go func() {
        someFunction(n, c)
    }()
    
    go func(num string) {
        someFunction(num, c)
    }(n)
//   /\    This will work fine, because 'n' is copied by value into 'num'
}
```