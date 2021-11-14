# Go

A Go package can be thought of as a project or a workspace. It's a collection of common source code files. A package can have many related files inside of it, with each file ending in the `.go` file extension. Each file in the package needs to have the statement `package PACKAGE_NAME` at the top (so `package examplePackage` for a package called `examplePackage`).

There are two types of packages: **executable** and **reusable**. **Executable** packages create an executable file once built. Reusable packages are like code dependencies or libraries, they contain reusable logic and helper functions which help us reuse code on future projects. The name of the package determines if a package is executable or reusable - a package is named `main` to make it an executable package, while any other name results in reusable packages. An executable package **must** have a function inside it called `main`, which runs automatically once the program is executed.

Packages can be imported using the `import "examplePackage"` command.

Functions are declared using the `func` keyword, followed by standard parentheses in which a list of arguments is specified, after which the curly braces which contain the function body are placed. Return types, if any, should be placed between the parentheses and the curly braces. The content which is returned from the function must match the type defined in the function prototype.
    
    func exampleFunction() {
        // This function doesn't return anything
    }
    
    //          Arguments   |     |   Return type
    //                     \/    \/
    func thisReturnsAString() string {
        return "This is a string"
    }
Variables in Go are static types. The type can be explicitly defined, or it can be automatically inferred from the value assigned during initialization. Another way to initialize a variable is by using the `:=` operator, which is somewhat shorter than the alternatives. The `:=` operator can only be used when a variable is initialized, it can't be used later on to assign a new value to it (the standard `=` is used then).
    
    // All these ways to initialize are equivalent
    var text string = "This is some text"
    var text = "This is some text"
    text := "This is some text"
Variables can be declared outside of a function, but values can't be assigned to them there.
    
    var exampleText string
    
    func main() {
    	exampleText = "test"
    	fmt.Println(exampleText)
    }
In Go, an **array** is a fixed length list of things, whereas a **slice** is an array that can grow or shrink. They must be defined with a data type, and every element inside of an array or a slice must be of the same type. Declaring a slice in Go can be done like so:
    
    // mySlice := []TYPE{ELEMENTS}
    myText := []string{"Text 1", textReturnFunction(), "Text 3"}
    // 'append' doesn't mutate the original, but returns a new slice
    myText = append(myText, "Text 4")
Iterating over a list of items can be done using `for` loops.
    
    // i - Counter/Index
    // card - Name which will be used to reference each element
    // cards - Collection of elements whose elements we're iterating over
    for i, card := range cards {
        fmt.Println(card)
    }
___
Go isn't an object oriented programming language, so there is no concept of classes in it. However, we can still make new types as specialized version of existing types - essentially adding custom functionality to existing types.
    
    // Package, imports, etc.
    
    // 'deck' will have the same properties as a string slice
    // Now instead of cards := []string{"Card 1", "Card2"}
    // We can do      cards := deck{"Card 1", "Card2"}
    type deck []string
    
    // Adds a receiver function called 'print' to all 'deck' variables
    // Invoked like so: cards.print()
    // 'd' is the reference to the variable the function is called on (cards)
    func (d deck) print() {
        for i, card := range d {
            fmt.Println(i, card)
        }
    }
The convention with receiver function arguments is for them to be 1-3 letters short, and to be an abbreviation of the actual thing they're supposed to be.


### Console commands
- `go` - Lists available commands
- `go build` - Compiles files
- `go run` - Compiles and executes files
- `go fmt` - Formats all the code in each file in the current directory
- `go install` - Compiles and "installs" a package
- `go get` - Downloads the raw source code of someone else's package
- `go test` - Runs any tests associated with the current project