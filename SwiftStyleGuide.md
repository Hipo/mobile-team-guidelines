**SWIFT STYLE GUIDE*

This document is powered by the document on [Swift Style Guide](https://google.github.io/swift/).

*SOURCE FILE BASICS*

*File Names*

1. A file containing a single type [MyType] -> MyType.swift
2. A file containing a type [MyType] and some top-level helper functions -> MyType.swift
3. A file containing a single extension to a type [MyType] that adds conformance to a protocol [MyProtocol] -> MyType+MyProtocol.swift
4. A file containing multiple extensions to a type [MyType] that add conformances, nested types, or other functionality -> More general name with prefix ‘MyType+’, i.e. MyType+Additions.swift 
5. A file containing related declarations that are not otherwise scoped under a common type or namespace -> Math.swift

*Whitespace Characters*

1. The line terminator and horizontal space character are the only whitespace characters that appears anywhere in a source file.
2. All other whitespace characters in string and character literals are represented by their corresponding escape sequence. Special escape sequences are used rather than the equivalent Unicode escape sequences. [\t ,\n ,\r ,\” ,\’ ,\\, \0]
3. Tab characters are not used for indentation.

*Invisible Characters and Modifiers*
Control characters, combining characters, and variation selectors that do affect the graphical representation of a string are not escaped when they are attached to a character or characters that they modify.

```swift
let size = "Übergröße" // Umlauts
let shrug = "🤷" // Specific combination rendered as a single character.
```

If such a Unicode scalar is present in isolation or is otherwise not modifying another character in the same string, it is written as a Unicode escape sequence.

```swift
let diaeresis = "\u{0308}" // NOT let diaeresis = "̈"
let skinToneType6 = "\u{1F3FF}" // NOT let skinToneType6 = "🏿"
```

*String Literals*
Unicode escape sequences (\u{????}) and literal code points (for example, Ü) outside the 7-bit ASCII range are never mixed in the same string.

```swift
let size = "Übergröße\n" ✔︎
let size = "\u{00DC}bergr\u{00F6}\u{00DF}e\n" ✔︎ but NOT preferable
let size = "Übergr\u{00F6}\u{00DF}e\n" ✘
```

*SOURCE FILE STRUCTURE*

*Import Statements*

1. A source file imports exactly the top-level modules that it needs; nothing more and nothing less. If a source file uses definitions from both UIKit and Foundation, it imports both explicitly; it does not rely on the fact that some Apple frameworks transitively import others as an implementation detail.
2. Imports of whole modules are preferred to imports of individual declarations or submodules.
3. Imports of individual declarations are permitted when importing the whole module would otherwise pollute the global namespace with top-level definitions.
4. Imports of submodules are permitted if the submodule exports functionality that is not available when importing the top-level module, i.e UIKit.UIGestureRecognizerSubclass. 
5. Import statements are not line-wrapped.
6. Import statements are the first non-comment tokens in a source file. They are grouped in the following fashion, with the imports in each group ordered lexicographically and with exactly one blank line between each group:
   - Module/submodule imports not under test
   - Individual declaration imports (class, enum, func, struct, var)
   - Modules imported with @testable (only present in test sources)

```swift
import CoreLocation
import MyThirdPartyModule
import SpriteKit
import UIKit

import func Darwin.C.isatty

@testable import MyModuleUnderTest
```

*Type, Variable, and Function Declarations*

1. In general, most source files contain only one top-level type. Exceptions are allowed when:
   - A class and its delegate protocol may be defined in the same file. 
   - A type and its small related helper types may be defined in the same file. Especially useful when using *fileprivate* to restrict some functionality or type.
2. The order of types, variables, and functions in a source file, and the order of the members of those types, can have a great effect on readability. However, there is no single correct recipe for how to do it. What is important is that each file and type uses some logical order, which its maintainer could explain if asked. When deciding on the logical order of members, it can be helpful for readers and future writers (including yourself) to use *// MARK:* comments to provide descriptions for that grouping.

```swift
class MyViewController: UIViewController {
	
		// MARK: LifeCycle

		override func viewDidLoad() {
		}

		override func viewWillAppear(_ animated: Bool) {
		}

		// MARK: Action

		@objc private func anyButtonWasTapped(_ sender: Any) {
		}

		// MARK: Notification

		@objc private func anyNotificationWasReceived(_ notification: Notification) {
		}
}
```

*Overloaded Declarations*
When a type has multiple initializers or subscripts, or a file/type has multiple functions with the same base name (though perhaps with different argument labels), and when these overloads appear in the same type or extension scope, they appear sequentially with no other code in between.

*Extensions*
Extensions can be used to organize functionality of a type across multiple “units”. As with member order, you must use some logical organizational structure that you could explain to a reviewer if asked.

*GENERAL FORMATTING*

*Column Limit*
Swift code has a column limit of 100 characters. Any exceptions that would exceed this limit must be line-wrapped as described in /Line-Wrapping/ section. Exceptions:

*  Lines where obeying the column limit is not possible without breaking a meaningful unit of text that should not be broken (for example, a long URL in a comment). 
*  import statements.
*  Code generated by another tool.

*Braces*
In general, braces follow Kernighan and Ritchie (K&R) style for non-empty blocks with Swift-specific rules:

*  No line break before the opening brace ’{‘, unless required by the rules in /Line-Wrapping/ section.
*  A line break after the opening brace ‘{‘, except
  *  in closures, where the signature of the closure is placed on the same line as the curly brace, if it fits, and a line break follows the in keyword. 
  *  where it may be omitted as described in /One Statement Per Line/ section.
  *  empty blocks may be written as {}.
*  A line break before the closing brace ‘}’, except where it may be omitted as described in /One Statement Per Line/ section, or it completes an empty block.
* A line break after the closing brace ‘}’, if and only if that brace terminates a statement or the body of a declaration. For example, an else block is written } else { with both braces on the same line.

*Semicolons*
Semicolons are not used either to terminate or separate statements.

*One Statement Per Line*
There is at most one statement per line, and each statement is followed by a line break, except when the line ends with a block that also contains zero or one statements.

```swift
guard let value = value else { return 0 }

defer { file.close() }

let squares = numbers.map { $0 * $0 }

var someProperty: Int { return otherObject.somethingElse() }

required init?(coder aDecoder: NSCoder) { fatalError("no coder") }
```

Wrapping the body of a single-statement block onto its own line is always allowed. 

*Line-Wrapping*
Line-wrapping is the activity of dividing code into multiple lines that might otherwise legally occupy a single line.

Many declarations (such as type declarations and function declarations) and other expressions (like function calls) can be partitioned into breakable units that are separated by unbreakable delimiting token sequences.

```swift
public func index<Elements: Collection, Element>(of element: Element, in collection: Elements) -> Elements.Index? where Elements.Element == Element, Element: Equatable {
  // ...
}
```

Unbreakable token sequences: 			

*  'public func index<'
* '>('
* ')->'
*  'where'

Breakable token sequences

* Elements: Collection, Element
*  of element: Element, in collection: Elements
*  Elements.Index?
*  Elements.Element == Element, Element: Equatable

According to these concepts, the rules for line-wrapping are:

1. If the entire declaration, statement, or expression fits on one line, then do that.
2. Comma-delimited lists are only laid out in one direction: horizontally or vertically. In other words, all elements must fit on the same line, or each element must be on its own line.
3. A line starting with an unbreakable token sequence is indented at the same level as the original line. 
4. A continuation line that is part of a vertically-oriented comma-delimited list is indented exactly +2 spaces from the original line.
5. When an open curly brace ‘{‘ follows a line-wrapped declaration or expression, it is on the same line as the final line unless that line is indented at +2 spaces from the original line.
6. If the generic constraint list exceeds the column limit when placed on the same line as the return type, then a line break is first inserted before the where keyword and the where keyword is indented at the same level as the original line.
7. If the generic constraint list still exceeds the column limit after inserting the line break above, then the constraint list is oriented vertically with a line break after the where keyword and a line break after the final constraint.

```swift
public func index<Elements: Collection, Element>( // 2
  of element: Element, // 2, 4
  in collection: Elements // 2, 4
) -> Elements.Index? // 3
where // 6
  Elements.Element == Element, // 2, 4, 7
  Element: Equatable // 2, 4, 7
{ // 5
  ...
}
```

*Function Declarations*

1. *modifiers func name<* generic arguments *>(* formal arguments *) throws ->* result *where* generic constraints {
   Bold components are unbreakable token sequences, whereas the others are breakable ones. The rules above are applied here.
2. 

```swift
protocol MyProtocol {
		func myClass(
			_ klass: MyClass,
			willDoSomethingTo value: MyValue) // The same line.
} 

protocol MyProtocol {
		func myClass(
			_ klass: MyClass,
			willDoSomethingTo value: MyValue
		) // Or its own line.
} 
```

3. If types are complex or deeply nested, individual elements in the arguments constraints lists or the return type may also need to be wrapped. However, typealiases are often a better way to simplify complex declarations whenever possible.

```swift
public func performanceTrackingIndex<Elements: Collection, Element> (
  of element: Element,
  in collection: Elements
) -> (
  Element.Index?,
  PerformanceTrackingIndexStatistics.Timings,
  PerformanceTrackingIndexStatistics.SpaceUsed
) {
  ...
}
```

*Types and Extension Declarations*
The following examples can apply equally to class, struct, enum, extension and protocol.

*modifiers class Name<* generic arguments *>:* superclass and protocols *where* generic constraints {
Bold components are unbreakable token sequences, whereas the others are breakable ones.

```swift
class MyClass:
  MySuperclass,
  MyProtocol,
  SomeoneElsesProtocol,
  SomeFrameworkProtocol
{
  ...
}

class MyContainer<Element>:
  MyContainerSuperclass,
  MyContainerProtocol,
  SomeoneElsesContainerProtocol,
  SomeFrameworkContainerProtocol
{
  ...
}

class MyContainer<BaseCollection>:
  MyContainerSuperclass,
  MyContainerProtocol,
  SomeoneElsesContainerProtocol,
  SomeFrameworkContainerProtocol
where BaseCollection: Collection {
  ...
}

class MyContainer<BaseCollection>:
  MyContainerSuperclass,
  MyContainerProtocol,
  SomeoneElsesContainerProtocol,
  SomeFrameworkContainerProtocol
where
  BaseCollection: Collection,
  BaseCollection.Element: Equatable,
  BaseCollection.Element: SomeOtherProtocolOnlyUsedToForceLineWrapping
{
  ...
}
```

*Function Calls*

1. When a function call is line-wrapped, each argument is written on its own line, indented +2 from the original line.

```swift
let index = index(
  of: veryLongElementVariableName,
  in: aCollectionOfElementsThatAlsoHappensToHaveALongName) // The same line.

let index = index(
  of: veryLongElementVariableName,
  in: aCollectionOfElementsThatAlsoHappensToHaveALongName
) // Or its own line.
```

2. If the function call ends with a trailing closure and the closure’s signature must be wrapped, then place it on its own line and wrap the argument list in parentheses to distinguish it from the body of the closure below it.

```swift
someAsynchronousAction.execute(withDelay: howManySeconds, context: actionContext) {
  (context, completion) in
  doSomething(withContext: context)
  completion()
}
```

*Control Flow Statements*
If, guard, while, and for

```swift
if aBooleanValueReturnedByAVeryLongOptionalThing() &&
   aDifferentBooleanValueReturnedByAVeryLongOptionalThing() &&
   yetAnotherBooleanValueThatContributesToTheWrapping() {
  doSomething()
}

if aBooleanValueReturnedByAVeryLongOptionalThing() &&
   aDifferentBooleanValueReturnedByAVeryLongOptionalThing() &&
   yetAnotherBooleanValueThatContributesToTheWrapping()
{
  doSomething()
}

if let value = aValueReturnedByAVeryLongOptionalThing(),
   let value2 = aDifferentValueReturnedByAVeryLongOptionalThing() {
  doSomething()
}

if let value = aValueReturnedByAVeryLongOptionalThing(),
   let value2 = aDifferentValueReturnedByAVeryLongOptionalThingThatForcesTheBraceToBeWrapped()
{
  doSomething()
}

guard let value = aValueReturnedByAVeryLongOptionalThing(),
      let value2 = aDifferentValueReturnedByAVeryLongOptionalThing() else {
  doSomething()
}

guard let value = aValueReturnedByAVeryLongOptionalThing(),
      let value2 = aDifferentValueReturnedByAVeryLongOptionalThing()
else {
  doSomething()
}

for element in collection
    where element.happensToHaveAVeryLongPropertyNameThatYouNeedToCheck {
  doSomething()
}
```

*Other Expressions*

1. When line-wrapping other expressions that are not function calls, the second line is indented exactly +2 from the original line.
2. When there are multiple continuation lines, indentation may be varied in increments of +2 as needed. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.
3. If there are many continuation lines caused by long wrapped expressions, consider splitting them into multiple statements using temporary variables when possible.

```swift
let result = anExpression + thatIsMadeUpOf * aLargeNumber +
  ofTerms / andTherefore % mustBeWrapped + ( // +2 spaces
    andWeWill - keepMakingItLonger * soThatWeHave / aContrivedExample) // +2 spaces, and not syntactically parallel with previous line.
```

*Horizontal Whitespace*
Horizontal Whitespace refers to interior space. 

Beyond where required by the language or other style rules, a single space also appears where:

1. Separating any reserved word starting a conditional or switch statement from the following expression which starts with an open parenthesis (().

```swift
if (x == 0 && y == 0) || z == 0 {
  ...
}
```

2. Before any closing curly brace ‘}’ that follows code on the same line, before any open curly brace ‘{‘, and after any open curly brace ‘{‘ that is followed by code on the same line.

```swift
let nonNegativeCubes = numbers.map { $0 * $0 * $0 }.filter { $0 >= 0 }
```

3. On both sides of any binary or ternary operator, including the “operator-like” symbols described
   * ‘=‘ used in assignment, initialization of variables/properties, and default arguments in functions.
   * ‘&’ used in a protocol composition type.
   * The operator symbol in a function implementing that operator.
   * ‘->’ preceding the return type of a function.
   * NO space on either side of ‘.’ used to reference value and type members.
   * NO space on either side of the ‘..<‘ or ‘…’ used in range expressions.

4. After, but not before, ‘,’ in parameter lists and in tuple/array/dictionary literals.
5. After, but not before, ‘:’ in 
   * Superclass/protocol conformance lists and generic constraints.
   * Function argument labels and tuple element labels.
   * Variable/property declarations with explicit types.
   * Shorthand dictionary type names.
   * Dictionary literals.
6. At least two spaces before and exactly one space after ‘//‘ that begins an end-of-line comment. 
7. Outside, but not inside, the brackets of an array or dictionary literals and the parentheses of a tuple literal.

*Horizontal Alignment*
Horizontal alignment is forbidden except when writing obviously tabular data where omitting the alignment would be harmful to readability. In other cases, horizontal alignment is an invitation for maintenance problems if a new member is introduced that requires every other member to be realigned.

```swift
✔︎           										✘
struct DataPoint {									struct DataPoint {
  var value: Int									  var value:        Int	
  var primaryColor: UIColor						  var primaryColor: UIColor
}                                            }
```

*Vertical Whitespace*
A single blank line appears where:

1. Between consecutive members of a type: properties, initializers, methods, enum cases, and nested types, EXCEPT THAT:
   - A blank line is optional between two consecutive stored properties or two enum cases whose declarations fit entirely on a single line. Such blank lines can be used to create logical groupings of these declarations.
   - A blank line is optional between two extremely closely related properties that do not otherwise meet the criterion above, i.e. a private stored property and a related public computed property.
2. Only as needed between statements to organize code into logical subsections.
3. Optionally before the first member or after the last member of a type.

*Parentheses*
Parentheses are not used around the top-most expression that follows an if, guard, while, or switch keyword.

*FORMATTING SPECIFIC CONSTRUCTS*

*Non-Documentation Comments*
Non-documentation comments always use the double-slash format ‘//’, never the C-style block format ‘/* … */’.

*Properties*

1. Local variables are declared close to the point at which they are first used to minimize their scope.
2. Every let or var statement (whether a property or a local variable) declares exactly one variable with the exception of tuples.

*Switch Statements*
Case statements are indented at the same level as the switch statement, and the statements inside the case blocks are then indented +2 spaces from that level.

```swift
switch order {
case .ascending:
  print("Ascending")
case .descending:
  print("Descending")
case .same:
  print("Same")
}
```

*Enum Cases*

1. In general, there is only one case per line in an enum. The comma-delimited form may be used only when:
   - None of the cases have associated values or raw values,
   - All cases fit on a single line,
   - The cases do not need further documentation because their meanings are obvious from their names.

```swift
public enum Token {
  case comma
  case semicolon
  case identifier
}

public enum Token {
  case comma, semicolon, identifier
}

public enum Token {
  case comma
  case semicolon
  case identifier(String)
}
```

2. When all cases of an enum must be indirect, the enum itself is declared indirect and the keyword is omitted on the individual cases.
3. When an enum case does not have associated values, empty parentheses are never present.
4. The cases of an enum must follow a logical ordering that the author could explain if asked. If there is no obviously logical ordering, use a lexicographical ordering based on the cases’ names.

```swift
public enum HTTPStatus: Int {
  case ok = 200

  case badRequest = 400
  case notAuthorized = 401
  case paymentRequired = 402
  case forbidden = 403
  case notFound = 404

  case internalServerError = 500
} 
```

*Trailing Closures*

1. Functions should not be overloaded such that two overloads differ only by the name of their trailing closure argument.

```swift
func greet(enthusiastically nameProvider: () -> String) {
	...
}

func greet(apathetically nameProvider: () -> String) {
  ...
}

greet { ... }  // ✘ Ambiguous use of 'greet'
```

```swift
func greetEnthusiastically(_ nameProvider: () -> String) {
  ...
}

func greetApathetically(_ nameProvider: () -> String) {
  ...
}

greetEnthusiastically { ... } ✔︎
greetApathetically { ... } ✔︎
```

2. If a function call has multiple closure arguments, then none are called using trailing closure syntax.

```swift
UIView.animate(
  withDuration: 0.5,
  animations: {
    ...
  },
  completion: { finished in
    ...
  })
```

3. If a function has a single closure argument and it is the final argument, then it is always called using trailing closure syntax, EXCEPT THAT:
   * Labeled closure arguments must be used to disambiguate between two overloads with identical arguments lists. 
   * Labeled closure arguments must be used in control flow statements where the body of the trailing closure would be parsed as the body of the control flow statement.

```swift
Timer.scheduledTimer(timeInterval: 30, repeats: false) { timer in
  ...
}

if let firstActive = list.first(where: { $0.isActive }) { // Trailing closure causes a compile error here.
  ...
}
```

4. When a function called with trailing closure syntax takes no other arguments, empty parentheses ‘()’ after the function name are never present.

```swift
let squares = [1, 2, 3].map { $0 * $0 } ✔︎
let squares = [1, 2, 3].map({ $0 * $0 }) ✘
let squares = [1, 2, 3].map() { $0 * $0 } ✘
```

*Trailing Commas*
Trailing commas in array and dictionary literals are required when each element is placed on its own line. Doing so produces cleaner diffs when items are added to those literals later.

```swift
let configurationKeys = [
  "bufferSize",
  "compression",
  "encoding",  ✔︎
]
```

*Numeric Literals*
It is recommended but not required that long numeric literals (decimal, hexadecimal, octal, and binary) use ‘_’ separator to group digits for readability. Recommended groupings are three digits for decimal, four digits for hexadecimal, four or eight digits for binary literals, or value-specific field boundaries when they exist (such as three digits for octal file permissions). Do not group digits if the literal is an opaque identifier that does not have a meaningful numeric value.

*Attributes*
Parameterized attributes, such as @availability(…) or @objc(…), are each written on their own line immediately before the declaration, and are lexicographically ordered.

```swift
@available(iOS 9.0, *)
public func coolNewFeature() {
  ...
}
```

*NAMING*
Apple’s official [Swift.org - API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) are considered part of this style guide.

*Identifiers*

1. Identifiers contain only 7-bit ASCII characters.
2. Unicode identifiers are allowed if they have a clear and legitimate meaning in the problem domain of the code base.

```swift
let deltaX = newX - previousX
let Δx = newX - previousX // Greek letters representing mathematical concepts
```

*Initializers*
Initializer arguments that correspond directly to a stored property have the same name as the property.

```swift
struct Person {
		let name: String

		init(name: name) { // NOT init(name otherName: String)
			self.name = name
		}
}
```

*Static and Class Properties*
Static and class properties that return instances of the declaring type are not suffixed with the name of the type.

```swift
class UIColor {
		class var red: UIColor { // NOT class var redColor: UIColor
			...
		}
}
```

*Global Constants*
Like other variables, global constants are lower camel case.

```swift
let secondsPerMinute = 60 ✔︎
let SecondsPerMinute = 60 ✘
let kSecondsPerMinute = 60 ✘
let gSecondsPerMinute = 60 ✘
let SECONDS_PER_MINUTE = 60 ✘
```

*Delegate Methods*

1. All methods take the delegate’s source object as the first argument.
2. For methods that take the delegate’s source object as their only argument:  
   * If the method returns Void (such as those used to notify the delegate that an event has occurred), then the method’s base name is the delegate’s source type followed by an indicative verb phrase describing the event. The argument is unlabeled. 

```swift
func scrollViewDidBeginScrolling(_ scrollView: UIScrollView)
```

* If the method returns Bool (such as those that make an assertion about the delegate’s source object itself), then the method’s name is the delegate’s source type followed by an indicative or conditional verb phrase describing the assertion. The argument is unlabeled.

```swift
func scrollViewShouldScrollToTop(_ scrollView: UIScrollView) -> Bool
```

* If the method returns some other value (such as those querying for information about a property of the delegate’s source object), then the method’s base name is a noun phrase describing the property being queried. The argument is labeled with a preposition or phrase with a trailing preposition that appropriately combines the noun phrase and the delegate’s source object. 

```swift
func numberOfSections(in scrollView: UIScrollView) -> Int
```

3. For methods that take additional arguments after the delegate’s source object, the method’s base name is the delegate’s source type by itself and the first argument is unlabeled. Then:
   * If the method returns Void, the second argument is labeled with an indicative verb phrase describing the event that has the argument as its direct object or prepositional object, and any other arguments (if present) provide further context.

```swift
func tableView(
  _ tableView: UITableView,
  willDisplayCell cell: UITableViewCell,
  forRowAt indexPath: IndexPath)
```

* If the method returns Bool, the second argument is labeled with an indicative or conditional verb phrase that describes the return value in terms of the argument, and any other arguments (if present) provide further context.

```swift
func tableView(
  _ tableView: UITableView,
  shouldSpringLoadRowAt indexPath: IndexPath,
  with context: UISpringLoadedInteractionContext
) -> Bool
```

* If the method returns some other value, the second argument is labeled with a noun phrase and trailing preposition that describes the return value in terms of the argument, and any other arguments (if present) provide further context.

```swift
func tableView(
  _ tableView: UITableView,
  heightForRowAt indexPath: IndexPath
) -> CGFloat
```

*PROGRAMMING PRACTICE*

*Compiler Warnings*
Code should compile without warnings when feasible. Any warnings that are able to be removed easily by the author must be removed. A reasonable exception is deprecation warnings, where it may not be possible to immediately migrate to the replacement API.

*Initializers*
The initializers declared by the special ExpressibleBy*Literal compiler protocols are never called directly.

```swift
struct Kilometers: ExpressibleByIntegerLiteral {
  init(integerLiteral value: Int) {
    ...
  }
}

let k1: Kilometers = 10 ✔︎
let k2 = 10 as Kilometers ✔︎
let k = Kilometers(integerLiteral: 10) ✘
```

*Properties*
The get block for a read-only computed property is omitted and its body is directly nested inside the property declaration.

```swift
var totalCost: Int {
  return items.sum { $0.cost }
}
```

*Types with Shorthand Names*

1. Arrays, dictionaries, and optional types are written in their shorthand form ([Element], [Key: Value], Wrapped?) whenever possible. The long forms (Array<Element>, Dictionary<Key, Value>, and Optional<Wrapped>) are only written when required by the compiler.
2. Void is a typealias for the empty tuple (). In function type declarations, the return type is always written as Void, never as (). In functions declared with the func keyword, the Void return type is omitted entirely.
3. Empty argument lists are always written as (), never as Void. (Void) has a different meaning, an argument list with a single empty-tuple argument.

```swift
func doSomething() {
  ...
}

let callback: () -> Void
```

*Optional Types*

1. Sentinel values are avoided when designing algorithms; for example, an “index” of −1 when an element was not found in a collection.
2. Optional is used to convey a non-error result that is either a value or the absence of a value. For example, when searching a collection for a value, not finding the value is still a valid and expected outcome, not an error.
3. Optional is also used for error scenarios when there is a single, obvious failure state; that is, when an operation may fail for a single domain-specific reason that is clear to the client.

```swift
struct Int17 {
  init?(_ string: String) { // Failed if the string does not represent a valid integer.
    ...
  }
}
```

4. Conditional statements that test that an Optional is non-nil but do not access the wrapped value are written as comparisons to nil.

```swift
if value != nil { ✔︎                    if let _ = value { ✘
  ...                                     ...
}                                       }
```

*Error Types*

1. Error types are used when there are multiple possible error states.
2. Throwing errors instead of merging them with the return type cleanly separates concerns in the API. Invalid inputs and invalid state are treated as errors.

```swift
struct Document {
  enum ReadError: Error {
    case notFound
    case permissionDenied
    case malformedHeader
  }

  init(path: String) throws {
    ...
  }
}

do {
  let document = try Document(path: "important.data")
} catch Document.ReadError.notFound {
    ...
} catch Document.ReadError.permissionDenied {
    ...
} catch {
    ...
}
```

Such a design forces the caller to consciously acknowledge the failure case by:

* wrapping the calling code in a do-catch block and handling error cases to whichever degree is appropriate,
* declaring the function in which the call is made as throws and letting the error propagate out,
* using try? when the specific reason for failure is unimportant and only the information about whether the call failed is needed.

3. In general, force-try! is forbidden. Only exception is that Force-try! is allowed in unit tests and test-only code. It is also allowed in non-test code when it is unmistakably clear that an error would only be thrown because of programmer error.

```swift
let regex = try! NSRegularExpression(pattern: "a*b+c?")
```

The NSRegularExpression initializer throws an error if the regular expression is malformed, but when it is a string literal, the error would only occur if the programmer mistyped it. There is no benefit to writing extra error handling logic here. If the pattern above were not a literal but instead were dynamic or derived from user input, try! should not be used and errors should be handled gracefully.

*Force Unwrapping and Force Casts*

1. Force-unwrapping and force-casting are strongly discouraged. 
2. It is allowed when it is extremely clear from surrounding code why such an operation is safe. A comment should be present that describes the invariant that ensures that the operation is safe.

```swift
let value = getSomeInteger()

// This force-unwrap is safe because `value` is guaranteed to fall within the
// valid enum cases because it came from some data source that only permits
// those raw values.
return SomeEnum(rawValue: value)!
```

3. Force-unwraps are allowed in unit tests and test-only code without additional documentation. This keeps such code free of unnecessary control flow.

*Implicitly Unwrapped Optionals*

1. Implicitly unwrapped optionals are inherently unsafe and should be avoided whenever possible in favor of non-optional declarations or regular Optional types.
2. Implicitly unwrapped optionals can also surface in Swift code when using Objective-C APIs that lack the appropriate nullability attributes. If possible, coordinate with the owners of that code to add those annotations so that the APIs are imported cleanly into Swift. If this is not possible, try to keep the footprint of those implicitly unwrapped optionals as small as possible in your Swift code; that is, do not propagate them through multiple layers of your own abstractions.
3. Implicitly unwrapped optionals are also allowed in unit tests.

*Access Levels*

1. Omitting an explicit access level is permitted on declarations. For top-level declarations, the default access level is internal. For nested declarations, the default access level is the lesser of internal and the access level of the enclosing declaration.
2. Specifying an explicit access level at the file level on an extension is forbidden. Each member of the extension has its access level specified if it is different than the default.

```swift
extension String {
  public var isUppercase: Bool {
    ...
  }
}
```

*Nesting and Namespacing*

1. Swift allows enums, structs, and classes to be nested, so nesting is preferred (instead of naming conventions) to express scoped and hierarchical relationships among types when possible. For example, flag enums or error types that are associated with a specific type are nested in that type.

```swift
class Parser {
  enum Error: Swift.Error {
    case invalidToken(String)
    case unexpectedEOF
  }

  func parse(text: String) throws {
    ...
  }
}
```

2. Declaring an enum without cases is the canonical way to define a “namespace” to group a set of related declarations, such as constants or helper functions.

```swift
enum Dimensions { // NO *struct* since it requires the boilerplate code to prevent instantiation.
  static let tileMargin: CGFloat = 8
  static let tilePadding: CGFloat = 4
  static let tileContentSize: CGSize(width: 80, height: 64)
}
```

*guard for Early Exists*
guard statements improve readability by eliminating extra levels of nesting (‘pyramid of doom); failure conditions are closely coupled to the conditions that trigger them and the main logic remains clear within its scope.

```swift
func discombobulate(_ values: [Int]) throws -> Int {
  guard let first = values.first else {
    throw DiscombobulationError.arrayWasEmpty
  }
  guard first >= 0 else {
    throw DiscombobulationError.negativeEnergy
  }

  var result = 0
  for value in values {
    result += invertedCombobulatoryFactory(of: value)
  }
  return result
}
```

*for-where Loops*
When the entirety of a for loop’s body would be a single if block testing a condition of the element, the test is placed in the where clause of the for statement instead.

```swift
for item in collection where item*.*hasProperty {
  ...
}
```

*fallthrough in switch Statements*

1. When multiple cases of a switch would execute the same statements, the case patterns are combined into ranges or comma-delimited lists.

```swift
switch value {
case 1: print("one")
case 2...4: print("two to four")
case 5, 7: print("five or seven")
default: break
}
```

2. There is never a case whose body contains /only/ the fallthrough statement.

```swift
switch value {
case 1: print("one")
case 2: fallthrough
case 3: fallthrough
case 4: print("two to four")
case 5: fallthrough
case 7: print("five or seven")
default: break
}
```

3. Cases containing /additional/ statements which then fallthrough to the next case are permitted.

*Tuple Patterns*
Assigning variables through a tuple pattern (sometimes referred to as a tuple shuffle) is only permitted if the left-hand side of the assignment is unlabeled.

```swift
let (a, b) = (y: 4, x: 5.0)
```

*Numeric and String Literals*
Integer and string literals in Swift do not have an intrinsic type. For example, 5 by itself is not an Int; it is a special literal value that can express any type that conforms to ExpressibleByIntegerLiteral and only becomes an Int if type inference does not map it to a more specific type.

1. When a literal is used to initialize a value of a type other than its default, and when that type cannot be inferred otherwise by context, specify the type explicitly in the declaration or use an as expression to coerce it.

```swift
let x1 = 50 // Int
let x2: Int32 = 50 // Int32
let x3 = 50 as Int32 // Int32
```

2. The compiler will emit errors appropriately for invalid literal coercions. They are good because the errors are caught at compile-time and for the right reasons.

```swift
// error: integer literal '9223372036854775808' overflows when stored into 'Int64'
let a = 0x8000_0000_0000_0000 as Int64
```

3. Using initializer syntax for these types of coercions can lead to misleading compiler errors, or worse, hard-to-debug runtime errors.

```swift
// This first tries to create an `Int` (signed) from the literal and then
// convert it to a `UInt64`. Even though this literal fits into a `UInt64`, it
// doesn't fit into an `Int` first, so it doesn't compile.
let a1 = UInt64(0x8000_0000_0000_0000)
```

*Playground Literals*
The graphically-rendered playground literals # colorLiteral(…), # imageLiteral(...), and # fileLiteral(...) are forbidden in non-playground production code. Use initialization methods.

*Defining New Operators*

1. In general, defining custom operators should be avoided. 
2. It is allowed when an operator has a clear and well-defined meaning in the problem domain and when using an operator significantly improves the readability of the code when compared to function calls.
3. If you must use third-party code of unquestionable value that provides an API only available through custom operators, you are strongly encouraged to consider writing a wrapper that defines more readable methods that delegate to the custom operators. This will significantly reduce the learning curve required to understand how such code works.

*Overloading Existing Operators*

1. Overloading operators is permitted when your use of the operator is semantically equivalent to the existing uses in the standard library.
2. If you wish to overload an existing operator with a meaning other than its natural meaning, follow the guidance in the “Defining New Operators” section to determine whether this is permitted.

*DOCUMENTATION COMMENTS*

*General Format*

1. Documentation comments are written using the format where each line is preceded by ‘///‘.
2. Javadoc-style block comments ‘/** … */‘ are not permitted.

*Single-Sentence Summary*

1. Documentation comments begin with a brief single-sentence summary that describes the declaration. 
2. If more detail is needed than can be stated in the summary, additional paragraphs (each separated by a blank line) are added after it. 
3. Method summaries are generally written as verb phrases without “this method […]” because it is already implied as the subject and writing it out would be redundant.

```swift
/// Returns the sum of the numbers in the given array.
///
/// - Parameter numbers: The numbers to sum.
/// - Returns: The sum of the numbers.
func sum(_ numbers: [Int]) -> Int {
  ...
}
```

*Parameters, Returns, and Throws Tags*

1. Clearly document the parameters, return value, and thrown errors of functions using the “Parameter(s)”, “Returns”, and “Throws” tags, in that order. None ever appears with an empty description. 
2. When a description does not fit on a single line, continuation lines are indented 2 spaces in from the position of the starting the tag.
3. The recommended way to write documentation comments in Xcode is to place the text cursor on the declaration and press *Command + Option + /*.
4. Parameter(s) and Returns tags may be omitted only if the single-sentence brief summary fully describes the meaning of those items and including the tags would only repeat what has already been said.
5. The content following the “Parameter(s)”, “Returns”, and “Throws” tags should be terminated with a period, even when they are phrases instead of complete sentences.
6. When a method takes a single argument, the singular inline form of the “Parameter” tag is used.
7. When a method takes multiple arguments, the grouped plural form “Parameters” is used and each argument is written as an item in a nested list with only its name as the tag.

```swift
/// Returns the output generated by executing a command.
///
/// - Parameter command: The command to execute in the shell environment.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String) -> String {
  ...
}

/// Returns the output generated by executing a command with the given string
/// used as standard input.
///
/// - Parameters:
///   - command: The command to execute in the shell environment.
///   - stdin: The string to use as standard input.
/// - Returns: A string containing the contents of the invoked process's
///   standard output.
func execute(command: String, stdin: String) -> String {
  ...
}
```

*Apple’s Markup Format*
Use of  [Apple’s markup format](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/)  is strongly encouraged to add rich formatting to documentation.

*Where to Document*
At a minimum, documentation comments are present for every open or public declaration, and every open or public member of such a declaration. EXCEPTIONS:

* Individual cases of an enum often are not documented if their meaning is self-explanatory from their name. Cases with associated values, however, should document what those values mean if it is not obvious.
* A documentation comment is not always present on a declaration that overrides a supertype declaration or implements a protocol requirement, or on a declaration that provides the default implementation of a protocol requirement in an extension unless the declaration describes new behavior from the one it overrides.
* A documentation comment is not always present on test classes and test methods.
* A documentation comment is not always present on an extension declaration. You may choose to add one if it help clarify the purpose of the extension, but avoid meaningless or misleading comments. 
* In general, if you find yourself writing documentation that simply repeats information that is obvious from the source and sugaring it with words like “a representation of,” then leave the comment out entirely.