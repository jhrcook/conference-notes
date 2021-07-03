# WWDC 2021 Notes

### Watch List

- [x] What‘s new in Swift
- [ ] What's new in Foundation
- [ ] What's new in watchOS 8
- [ ] ARC in Swift: Basics and beyond
- [x] Meet DocC documentation in Xcode
- [ ] Build interactive tutorials using DocC
- [ ] Host and automate your DocC documentation
- [x] Meet async/await in Swift
- [ ] Protect mutable state with Swift actors
- [ ] Explore structured concurrency in Swift
- [ ] Meet AsyncSequence
- [ ] Use async/await with URLSession
- [x] What's new in SwiftUI
- [x] Demystify SwiftUI
- [ ] Add rich graphics to your SwiftUI app
- [x] Direct and reflect focus in SwiftUI
- [ ] Discover concurrency in SwiftUI
- [x] SwiftUI on the Mac: Build the fundamentals
- [x] SwiftUI on the Mac: The finishing touches
- [ ] Swift concurrency: Update a sample app
- [ ] Swift concurrency: Behind the scenes
- [ ] Use the camera for keyboard input in your app
- [ ] Ultimate application performance survival guide
- [ ] Meet the Screen Time API
- [x] What’s new in SF Symbols
- [x] SF Symbols in SwiftUI
- [ ] Discover built-in sound classification in SoundAnalysis
- [ ] Bring Core Data concurrency to Swift and SwiftUI
- [ ] Build a workout app for Apple Watch
- [ ] Build dynamic iOS apps with the Create ML framework
- [ ] Classify hand poses and actions with Create ML
- [x] Craft search experiences in SwiftUI
- [ ] Detect people, faces, and poses using Vision
- [ ] Extract document data using Vision
- [ ] Tune your Core ML models
- [ ] Use async/await with URLSession
- [ ] Detect and diagnose memory issues
- [ ] Detect bugs early with the static analyzer
- [ ] Diagnose Power and Performance regressions in your app
- [ ] Diagnose unreliable code with test repetitions
- [ ] Meet the Swift Algorithms and Collections packages
- [ ] Ultimate application performance survival guide

---

## What‘s new in Swift

[Recording](https://wwdc.io/share/wwdc21/10192)

> Join us for an update on Swift.
> Discover the latest language advancements that make your code easier to read and write.
> Explore the growing number of APIs available as Swift packages.
> And we’ll introduce you to Swift’s async/await syntax, structured concurrency, and actors.

### New packages

[**Swift Collections**](https://github.com/apple/swift-collections)

- contains (non-exhaustive list):
    - `Deque`
    - `OrderedSet`
    - `OrderedDictionary`
- more information in WWDC session: *Meet the Swift Algorithms and Collections packages*

[**Algorithms**](https://github.com/apple/swift-algorithms)

- colelction of sequence and collection algorithms
- more information in WWDC session: *Meet the Swift Algorithms and Collections packages*
- contains (non-exhaustive list):
    - generating combinations or permutations
    - iterating by groups of 2 or 3 or grouped by a predicate
    - get the 5 smallest/largest elements or just 5 at random

[**Swift System**](https://github.com/apple/swift-system)

Better functions for manipulting file names on macOS, Linux, or Windows

[**Swift Numerics**](https://github.com/apple/swift-numerics)

- added Float16 support on Apple silicon Macs
- support Float16 in numeric functions (e.g. $\log$)

[**ArgumentParser**](https://github.com/apple/swift-argument-parser)

- `Fish` shell completion
- joined short options
- improved error messages

### Swift on server

### Developer experience improvements

- introducing "Swift DocC"
    - multiple WWDC sessions (including on hosting and "building interactive tutorials using DocC")

### Build Improvements

- some changes to ARC that makes it more aggresive
    - option in Xcode to turn in on

### Ergonomic Improvements to Swift

- "result builders" have been brought to Swift (from SwiftUI)
    - WWDC session: *Write a DSL in Swift using result builders*
- imporvements to the `Codable` protocol
- "flexible static member lookup":

```swift
protocol Coffee { ... }
struct RegularCoffee: Coffee {}
struct Cappuccino: Coffee {}

extension Coffee where Self == Cappucino {
	static var cappucino: Cappuccino = { Cappuccino() }
}

func brew<CoffeeType: Coffee>(_ coffee: CoffeeType) { ... }

brew(.cappucino.large)
```

- can use property wrappers on function and closure arguments:

```swift
@propertyWrapper
struct NonEmpty<Value: Collection> {
	init(wrappedValue: Value) {
		precondition(!wrappedValue.isEmpty)
		self.wrappedValue = wrappedValue
	}

	var wrappedValue: Value {
		willSet { precondition(!newValue.isEmpty) }
	}
}

func logIn(@NonEmpty _ username: String) {
	print("Logging in: \(username)")
}
```

- relaxed `#if` syntax

```swift
import SwiftUI

struct SettingsView: View {
	@State var settings: [Setting]
	private let padding = 10.0
	var body: some View {
		List(0 ..< settings.count) { index in
			Toggle(settings[index].displayName, isOn: $settings[index].isOn)
			#if os(macOS)
			.toggleStyle(CheckboxToggleStyle())
			#else
			.toggleStyle(SwitchToggleStyle())
			#endif
		}
	}
}
```

- various other updates to SwiftUI

### Asynchronous and concurrent programming

- WWDC sessions for `async` and `await`:
    - *Meet async/await in Swift*
    - *Swift concurrency: Behind the scenes*
- WWDC sessions for concurrency:
    - *Explore structured concurrency in Swift*
- example of concurrency below:

```swift
func titleImage() async throws -> Image {
	async let background = renderBackground()
	async let foreground = renderForeground()
	async let title = renderTitle()
	return try await merge(background, foreground, title)

}
```

- new `actor` consturct protects data shared by multiple threads
    - reference objects (like classes) but handles async and concurrency
    - WWDC session: *Protect mutable state with Swift actors*

```swift
actor Statistics {
	private var counter: Int = 0
	func increment() {
		counter += 1
	}
	func publish() async {
		await sendResults(counter)
	}
}

var statistics = Statistics()
await statistics.increment()
```

## Meet DocC documentation in Xcode

[Recording](https://wwdc.io/share/wwdc21/10166)

> Discover how you can use DocC to build and share documentation for Swift packages and frameworks.
> We’ll show you how to begin generating documentation from your own code — or from third-party code you depend upon — and write and format it using Markdown.
> And we’ll also take you through the export process, helping you generate DocC archives to share with the public.

### Notes

- for swift packages and frameworks
- Xcode 13 comes with a "compiler" for DocC
- multiple parts:
    - "Reference": standard documentation for the source code
    - "Articles" for walking users through how to use the package/framework
    - "Tutorials" for walking through a project that uses the package/framework
- documetation for public APIs is automatic
- can have additional documenations in the "DocC Catalog"
    - more in WWDC session: *Elevate your DocC documentation in Xcode*
- how to build the doc:
    - menu access: Product > Build Documentation
    - for frameworks, there is a build setting: Build Settings / Documentation Compiler - Options / Build Documentation during 'Build'
    - support for `xcodebuild docbuild`
- supports standard Markdown formatting
- Xcode can add templates for documetation to functions
- connecting pieces of documentation
    - make connections using *double backtick* syntax: ``otherFunc(x)``
    - Xcode has code completion within the double backticks
    - reference propertiers and methods of a class using forward slashes: ``MyClass/classProperty``
- sharing documentation:
    - Xcode builds a "Documentation archives" that is a single file web app
    - WWDC session: *Host and automate your DocC documentation*
- can export and share documentation archive from Xcode's documentation viewer, too
- other sessions:
    - *Elevate your DocC documentation in Xcode*: picks up where this session left off
    - *Host and automate your DocC documentation*: how to build and host documentation
    - *Build interactive tutorials in DocC*: for building tutorials with step-by-step instructions for your package/framework

## Meet async/await in Swift

[Recording](https://wwdc.io/share/wwdc21/10132)

> Swift now supports asynchronous functions — a pattern commonly known as async/await.
> Discover how the new syntax can make your code easier to read and understand.
> Learn what happens when a function suspends, and find out how to adapt existing completion handlers to asynchronous functions.

- previously, many asynchronous tasks used completion handlers or delegate callbacks
    - often involved multiple nested completion handlers
    - tricky because need to make sure the completion handlers are called no matter the outcome of the actions in the function

```swift
func fetchThumbnail(for id: String, completion: @escaping (UIImage?, Error?) -> Void)
```

- below is an example of a throwing and asynchronous function for getting an image and making a thumbnail
    - mark function as `async` and `throws`
    - no longer need to handle URLSession's errors, just mark as `try` and an error will be propagated automatically

```swift
func fetchThumbnail(for is: String) async throws -> UIImage {
	let request = thumbnailURLRequest(for: id)  // blocking but fast
	// Asynchronously get the data using `URLSession` and throw an error if it fails.
	let (data, response) = try await URLSession.shared.data(for: request)
	// If bad HTTP response, through an error.
	guard (response as? HTTPURLResponse)?.statusCode == 200 else { throw FetchError.badID }
	// Attempt to convert the image to a `UIImage` (may return `nil`).
	let maybeImage = UIImage(data: data)
	// Asynchronously create the thumbnail and throw an error if it returns `nil`.
	guard let thumbnail = await maybeImage?.thumbnail else { throw FetchError.badImage }
	return thumbnail
}
```

- call `await maybeImage?.thumbnail` because the `.thumbnail` *property* is asynchronously
    - only *read-only* properties can be `async`
    - is implemented as an extension to `UIImage`:

```swift
extension UIImage {
	var thumbnail: UIImage? {
		get async {
			let size = CGSize(width: 40, height: 40)
			return await self.byPreparingThumbnail(ofSize: size)
		}
	}
}
```

- can use `await` in for-loops of asynchronous sequences
    - WWDC session: *Meet AsyncSequence*
    - WWDC session: *Explore structured concurrency in Swift*

```swift
for await id in staticIDsURL.lines {
	let thumbnail = await fetchThumbnail(for: id)
	collage.add(thumbnail)
}
let result = await collage.draw()
```

- keep in mind that the state of the application can change while an asynchronous function is suspended
    - function may also resume on a entirely different thread
    - WWDC session: *Protect mutable state with Swift actors*
- important notes to remember about async/await
    - `async` enables a function to suspend
    - `await` marks where an async function may suspend execution
    - other work can happen during a suspension
    - once the awaited async call completes, execution resumes after the `await`
- testing async code is built into XCTest

```swift
class MockViewModelSpec: XCTestCase {
	func testFetchThumbnails() async throws {
		XCTAssertNoThrow(try await self.mockViewModel.fetchThumbnail(for: mockID))
	}
}
```

- example of using `async` function in SwiftUI
    - the `.onAppear()` modifer does not take async methods
    - thus, need to use the `async { ... }` ("async task function") to provide an asynchronous context within a synchronous context
    - WWDC session: *Discover concurrency in SwiftUI*

```swift
struct ThumbnailView: View {
	@ObservedObject var viewModel: ViewModel
	var post: Post
	@State private var image: UIImage?

	var body: some View {
		Image(uiImage: self.image ?? placeholder)
			.onAppear {
				async {
					self.image = try? await self.viewModel.fetchThumbnail(for: post.id)
				}
			}
	}
}
```

- can "bridge" a function/method that still uses a completion handler into async using `withCheckedContinuation()` and `withCheckedThrowingContinuation()`
    - these are called with `await` and take a closure with a parameter `continuation: CheckedContinuation<Success, Error>`
    - the completion handler can be run within this closure and the results are sent back using `continuation.resume(returning: result)` or `continuation.resume(throwing: error)`
    - continuations must be resume *exactly* once
        - Swift will check for this, too

    -

```swift
func persistentPosts() async throws -> [Post] {
	// A simple typealias for this specific bridge.
	typealias PostContinuation = CheckedContinuation<[Post], Error>
	// Call `await` to spwan the asynchronous task.
	return try await withCheckedThrowingContinuation { (continuation: PostContinuation) in
		// Can now call the original completion handler asynchronously and return
		// the results when finished.
		self.getPersistentPosts {posts, error in
			if let error = error {
				continuation.resume(throwing: error)
			} else {
				continuation.resume(returning: posts)
			}
		}
	}
}
```

## What's new in SwiftUI

[Recording](https://wwdc.io/share/wwdc21/10018)

> There’s never been a better time to develop your apps with SwiftUI.
> Discover the latest updates to the UI framework — including lists, buttons, and text fields — and learn how these features can help you more fully adopt SwiftUI in your app.
> Find out how to create beautiful, visually-rich graphics using the canvas view, materials, and enhancements to symbols.
> Explore multi-column tables on macOS, refinements to focus and keyboard interaction, and the multi-platform search API.
> And we’ll show you how to take advantage of features like Swift concurrency, a brand new AttributedString, format styles, localization, and so much more.

### Better lists

- built in support for loading images asynchoronously (`AsyncImage()`)
    - can be given a URL directly
    - can modify the image in place
    - has a default placeholder or can specify one

```swift
ScrollView {
	LazyVGrid(columns: [GridItem(.adaptive(minimum: 420))]) {
		ForEach(photos) { photo in
			AsyncImage(url: photo.url) { image in
				image
					.resizable()
					.aspectRatio(contentMode: .fill)
			} placeholder: {
				randomColor(forURL: photo.url)
			}
			.frame(width: 400, height; 266)
			.mask(RoundedRectable(cornerRadius: 16))
		}
	}
	.padding()
}
```

- pull to refresh with `.refreshable { ... }` modifier
    - can be asynchronous with `await` on commands in closure
- new `.task()` modifier: attach async task to lifetime of the view
    - the task begins when the view loads and is canceled when the view is gone
    - ex: task for updating when an `AsyncSequence` has a new element
- interactive collections: pass a binding to a list to `List($myList)` and will return a binding to each element in the closure
    - can use with `ForEach`, too

```swift
struct DirectionsList: View {
	@Binding var directions: [Direction]

	var body: some View {
		List($directions) { $direction in
			Label {
				Text(direction.text)
			} icon: {
				DirectionsIcon(direction)
			}
		}
	}
}
```

- visually customize lists
    - `.listRowSeparatorTint()` modifier
    - `.listRowSeparator(.hidden)` to hide separators
- can define completely custom swipe actions with the `.swipeActions()` modifier
    - both leading and trailing
    - available on any platform with swipe actions

### Beyond lists

- new `Table()` view for macOS with similar behaviour as `List()` but can havle multiple columns
    - supports single and multiple row selection (the `selection` variable in the example)
    - supports sorting using key paths (the `sortOrder` variable in the example)

```swift
@State private var sortOrder = [KeyPathComparator(\StoryCharacter.name)]
@State private var selection = Set<StoryCharacter.ID>()

Table(characters, selection: $selection, sortOrder: $sortOrder) {
	TableColumn("◇") { CharacterIcon($0) }
		.width(20)
	TableColumn("Villain") { Text($0.isVillain ? "Villain" : "Hero") }
		.width(40)
	TableColumn("Name", value: \.name)  # use key path for `.name` for sorting
	TableColumn("Name", value: \.powers)
}
```

- specific SwiftUI operability with CoreData in `List()` and `Table()`.
    - fetch requests provide a binding to their sort descriptors
    - also for sectioned fetch requests so can have sectioned lists with the data driven by CoreData
- WWDC sessions for SwftUI with CoreData and macOS:
    - *SwiftUI on the Mac: Build the fundamentals*
    - *SwiftUI on the Mac: The finishing touches*
    - *Bring Core Data concurrency to Swift and SwiftUI*
- add search using the `searchable()` modifer
    - automatically adds a search bar to the appropriate location and will have search suggestions
    - WWDC session: *Craft search experiences in SwiftUI*

```swift
NavigationView {
	List {
		...
	}
	Text("No selection")
}
.searchable(text: $characters.filterText)
```

- additional features for sharing data on macOS:
    - the `.onDrag()` modifier now has a `preview: { ... }` parameter to specify what the draged objects looks like
    - `.importsItemProviders()` allows addition of `ItemProviders` (such as dragging images, I think)
        - can also add the `ImportFromDevicesCommands()` to a the commands for a `WindowGroup` to allow importing data from other devices (e.g. taking a photo on a phone and automatically passing it to the phone)
    - exporting data with `.exportsItemProviders()` modifier
        - allows for integration with other services such as Shortcuts

### Advanced graphics

- new SF Symbols and hierarchcical colorations
    - SwiftUI will add modifiers automatically to SF Symbols depending on the context
        - e.g.: will automatically use the ".fill" version of symbols in icons in tab views
    - WWDC sessions
        - *What’s new in SF Symbols*
        - *SF Symbols in SwiftUI*
- new `Canvas()` view: immediate drawing for showing many graphics that do *not* require individual tracking
    - available on all platforms

```swift
Canvas { context, size in
	let metrics = gridMetrics(in: size)
	for (index, symbol) in symbols.enumerated() {
		let rect = metrics[index]
		let image = context.resolve(symbol.image)
		context.draw(image, in: rect.fit(image.size))
	}
}
```

- new `TimelineView()` for updating over time based on a schedule
    - with watchOS 8, better tools for dimming the always-on display and can use `TimelineView()` to provide information on how to update the dimmed display over time
    - various different schedules: `.everyMinute`, `.periodict(from: Date, by: TimeInterval)`, `.explicit([Date])`, `.animation`
    - or make a custom schedule with `struct CustomSchedule: TimelineSchedule`
    - can add `.privacySensitive(true)` modifier to hide sensitive info when dimmed or on Widgets
    - WWDC session: *What's new in watchOS 8*
- new "materials" for backgrounds (e.g. `.regularMaterial`, `.ultraThinMaterial`)
- new `.safeAreaInset()` modifier to add views to bottom of screens
    - works well with `ScrollView` to make sure content is not blocked
    - WWDC session: *Add rich graphics to your SwiftUI app*
- additional accessibility helpers in SwiftUI previews in Xcode
    - WWDC session: *SwiftUI accessibility: Beyond the basics*

### Text and keyboard (and focus)

- `Text` has direction Markdown support: `Text("**Hello**, World!")`
    - can also add custom modifications using new features of `AttributedString`
- `Text` has automatic localization of text
    - WWDC session: *Localize your SwiftUI app*
- can restrict dynamic text ranges
- allow text selection with the `.textSelection(.enabled)` modifier
    - on all platforms
- some changes to data formatting
- `TextField` can now have restrictions on the input and various formatting options
    - can add prompts, too

```swift
@State private var newAttendee = PersonNameComponents()

var body: some View {
	...
	TextField("New Person", value: $newAttendee, format: .name(style: .medium))
}
```

- keyboard:
    - new `.onSubmit()` modifier for `TextField` can run commands after keyboard indicates text submission
        - has new `.submitLabel(.done)` modifier
    - new `ToolbarItemGroup(placement: .keyboard) { ... }` to add buttons to the top of keyboards (or to TouchBar on macOS)
- `@FocusState`: new property wrapper to control or indicate focus
    - often will be a boolean, but can take any hashable type
    - can be used to dismiss the keyboard by setting to `nil` when aditing a `TextField`
    - WWDC session: *Direct and reflect focus in SwiftUI*

```swift
private enum Field: Int, Hashable {
	case name, location, date, addAttendee
}

@FocusState private var focusedField: Field?

var body: some View {
	...
	TextField("New Person", value: $newAttendee, format: .name(style: .medium))
		.focused($focusedField, equals: .addAttendee)
	...
	Button {
		// Set `TextField` for name of new attendee when the button is pressed.
		focusedField = .addAttendee
	} label: {
		Label("Add Attendee", systemImage: "plus")
	}
}
```

### More buttons

- new modifiers:
    - standard bordered button style: `.buttonStyle(.bordered)`
    - `.tint(.green)`: tint for a button
    - `.controlSize(.small)`: size of the button
    - `.controlProminence(.increased)`: adjusts to tint
- changes to `Menu` buttons (show drop downs for multiple actions)
    - can hide the indicator with `.menuIndicator(.hidden)`
    - can add primary action with `.primaryAction()`
        - click does primary action and longer press/click shows menu

```swift
Menu("Add") {
	ForEach(jarstore.allJars) { jar in
		Button("Add to \(jar.name)") {
			jarStore.add(buttonEntry, to: jar)
		}
	}
} primaryAction: {
	jarStore.addToDefaultJar(buttonEntry)
}
.menuStyle(.button)
```

- new `Toggle` style to look like a button: `.toggleStyle(.button)`
    - looks like a button but behaves like a toggle
- `ControlGroup` to group buttons and toggles

## Demystify SwiftUI

[Recording](https://wwdc.io/share/wwdc21/10022)

> Peek behind the curtain into the core tenets of SwiftUI philosophy: Identity, Lifetime, and Dependencies.
> Find out about common patterns, learn the principles that drive the framework, and discover how you can use them to guarantee correctness and performance for your app.

### Overview

- SwiftUI looks for the following 3 things in code:
  1. *Identity*: elements as the same or distinct across multiple updates of the app
  2. *Lifetime*: tracking existence of views and data over time
  3. *Dependencies*: how SwiftUI knows when to update and why

### Identity

- given 2 views, are they completely different or are they the same view in a different state
    - how SwiftUI knows how to transition between views
- types of identity:
    1. *explicit*: custom or data driven identifiers
        - e.g. having two dogs with different names
        - e.g. the `id` parameter in `ForEach()`
        - e.g. the `.id()` modifier on a SwiftUI view
    2. *structural*: distinguishing views by their type and position in view hierarchy
        - structure of the view hierarchy to create implicit identifiers
        - e.g. if have two dogs next to each other, can refer to them as "the one on the left" or "right"
- example of structural identity causing different bahviour in SwiftUI

```swift
// Causes the green and red `PawViews` to fade in or out depending on `dog.isGood`.
VStack {
    if dog.isGood {
        PawView(tint: .green)
        Spacer()
    } else {
        Spacer()
        PanViiw(tint: .red)
    }
}

// Causes color to change and `PawView` to move depending on `dog.isGood`.
PawView(tint: dog.isGood ? .green : .red)
    .frame(
        maxHeight: .infinity,
        alignment: dog.isGood ? .top : .bottom
    )
```

- nemesis of Identity in SwiftUI: `AnyView`
    - "type-erasing wrapper type"
    - avoid at all costs!

### Lifetime

- different *values* for a single *element* over time
    - a view's values are *not* the same as the view's identity
    - the value changes, but SwiftUI sees it as the same element
- `@State` and `@StateObject` tell SwiftUI that it will need to keep those elements for the view's lifetime
    - identity of the data can be used as the identity of the view by SwiftUI

### Dependencies

- a dependency is just an input to a view (the view's properties)
    - when a dependency changes, the view must produce a new `body`
- SwiftUI builds a dependency graph so it can tell what needs to be updated when a dependency changes
    - the identities of the views are used to locate an element even as the values change

### Identifier stability

- the stability of the identity of an element determines its lifetime
- directly affects performance
    - needlessly changing identity can result in "dependency churn"
- example of an unstable identifier
    - the ID changes every time any of the data changes
    - adding a new element to `pets` causes all of the data to change because each element will have a different value for `id`

```swift
enum Animal { case dog, cat }

struct Pet: Identifiable {
    var name: String
    var kind: Animal
    var id: UUID { UUID() }  // unstable ID
}

struct FavoritePets: View {
    var pets: [Pet]
    var body: some View {
        List {
            ForEach(pets) {
                PetView($0)
            }
        }
    }
}
```

- another example where the identity of the elements in `pets` is tied to its index
    - inserting a new pet at index 0, would actually cause a new element to appear at the bottom
    - the identity of each pet would increment, causing SwiftUI to think they are all new

```swift
struct FavoritePets: View {
    var pets: [Pet]
    var body: some View {
        List {
            ForEach(pets.indices, id: \.self) {  // unstable ID
                PetView(pets[$0])
            }
        }
    }
}
```

### Identifier uniqueness

- improves animations, performance, and accuracy of SwiftUI's dependency graph
- example with a bug where identifiers would not be unique
    - cannot have a `Treat` in `treats` with the same `.name`

```swift
struct TreatJar: View {
    var treats: [Treat]
    var body: some View {
        ScrollView {
            LazyVGrid(...) {
                ForEach(treats, id: \.name) {  // non-unique ID
                    TreatCell($0)
                }
            }
        }
    }
}
```

### Structural identity

- below is an example with a bug where an element can accidentally have a different structural identity
    - fix by moving conditional into the opacity modifier: `.opacity(date < .now ? 0.3 : 1.0)`

```swift
ForEach(treats, id: \.serialNumber) {  // good unique ID
    TreatCell($0)
    .modifier(ExpirationModifier(date: treat.expiryDate))
}

struct ExpirationModifier: ViewModifier {
    var date: Date
    func body(content: Content) -> some View {
        if date < .now { // branch causes different structural ID based on `date`
            content.opacity(0.3)
        } else {
            content
        }
    }
}
```

- `.opacity(1.0)` is an "Inert modifier" that is actually removed by SwiftUI before rendering
    - some examples of inert modifiers that should be used instead of conditional branches like the above example
        1. `opacity(1.0)`
        2. `padding(0)`
        3. `transformEnvironment(...) {}`: conditional writing to the environment

## What’s new in SF Symbols

[Recording](https://wwdc.io/share/wwdc21/10097)

> Explore the latest updates to SF Symbols, Apple’s iconography library.
> Designed to integrate seamlessly with San Francisco — the system font for Apple platforms — SF Symbols can help you create beautiful and consistent iconography for your app while supporting accessibility features like Dynamic Type and Bold Text.
> Discover the latest additions to the SF Symbols library, localization enhancements, and how you can more easily customize the color of a symbol to integrate it within your app’s own color palette.
> We’ll also show you how you can design and annotate custom symbols to support Monochrome, Hierarchical, Palette, and Multicolor rendering modes.

- about 600 new symbols for this year (total 3,000)
    - Apple products
    - game controllers
    - some specific to watchOS
- localization
    - additional languages in iconsthat have characters
    - update automatically based on localization of user
- anotomy of the symbol
    - every symbol starts with a point
    - then can either draw a line or curve (brezier control points)
    - build an icon from paths (completely enclosed shape made up of segments)
    - expand to different formats (e.g. bold) while maintaining "point compatability" (same number of vector points per icon)
    - interpolate between the instances to get all weights and scales for SF Symbols
    - add annotations to a symbol to create hierarchy from foreground to background
        - allows for unique highlighting and coloration of different parts of the symbol
- rendering modes
    - "Hierarchical": different shades (single color with different opacities) for foreground and background
    - "Palette": like hierarchical but with multiple colors
        - meant to convey similarities between icons (e.g. groups of icons)
        - can control opacities
    - "Multicolor": multiple colors for different parts of icon
        - meant to convey either real-world coloration of an object or some other obvious meaning (e.g. green "plus" sign on document icon to convey adding a new document)
        - can have one part of an icon in Multicolor (like a "plus" on the corner of another icon) and the rest be the default Accest color of the application
        - some icons come with multicoloration by default
    - "Monochrome": single color
        - color and opacity controls, but same throughout entire icon (does not use hierarchy annotation)
        - useful when displaying many icons and do not need to emphasize one over the rest
- changes to SF Symbols app
    - can play with the different render modes and different colors
    - additional color palettes: "Light" and "Dark" (already existent), and "Light increased contrast" and "Dark increased contrast"
        - available on all platforms, but some may be tweaked for specific platforms

## SF Symbols in SwiftUI

[Recording](https://wwdc.io/share/wwdc21/10349)

> Discover how you can incorporate SF Symbols into your SwiftUI app.
> We’ll explore basic techniques for presenting symbols, customizing their size, and showing different variants.
> We’ll also take you through the latest updates to symbol colorization and help you pick the right tool for your app’s needs.

```swift
// Basic use
Image(systemName: "heart")
Label("Heart", systemImage: "heart")
```

```swift
// Add Accessibility label for voice over.
Image(systemName: "person.circle").accessibilityLabel("Profile")
```

- can use SFSymbol in string interpolation

```swift
Text("""
    Thalia, Paul, and
    3 others \(Image(systemName: "chevron"))
""")
```

- coloration

```swift
.foregroundStyle(.red)        // color red
.foregroundStyle(.tint)       // default app tint color
.foregroundStyle(.secondary)  // secondary color determined by system
```

- font modifier to control style and size
    - use a font style will let the image scale using dynamic type

```swift
.font(.body)
.font(.caption)
.font(.system(size: 10))
```

- change size relative to the font size of the text
    - does not change the size of the text

```swift
.imageScale(.large)  // or `.medium`, `.small`
```

- variants (such as ".fill" symbols)
    - now automatically applied by the system depending on the context
    - e.g. SwiftUI will automatically use the filled version for tab bars in iOS
    - can also apply different variants using a SwiftUI modifier: `.symbolVariant()`
        - can use with custom symbols, too, as long as the variants follow the same naming conventions as SwiftUI

```swift
List {
    Label("Ace of Hearts", systemImage: "suit.heart")
    Label("Ace of Spades", systemImage: "suit.spade")
}
.symbolVariant(.circle.fill)
```

- rendering modes: *Monochrome*, *Multicolor*, *Hierarchical*, *Palette*
    - details on each mode in WWDC session: *What's New in SF Symbols*
    - control with modifier: `symbolRenderingMode()`
    - can see which rendering modes a symbol supports in the SF Symbols App

```swift
List {
    Label("Ace of Hearts", systemImage: "suit.heart")  // red
    Label("Ace of Spades", systemImage: "suit.spade")  // black
}
.symbolVariant(.fill)
.symbolRenderingMode(.multicolor)
```

```swift
HStack {
    Button(action: moveToTop) {
        Image(systemName: "square.3.stack.3d.top.fill")
    }
    Button(action: moveToBottom) {
        Image(systemName: "square.3.stack.3d.bottom.fill")
    }
}
.symbolRenderingMode(.hierarchical)  // different opacities to parts of symbols
```

- can provide specific colors for the multicolor mode
    - which layers a symbol has depends on the specific variant (e.g. `.circle` vs. `.fill`)
    - can provide one color for each level, or just two for Primary and all the rest
    - can also provide different *styles* instead of just a color (see commented out example)
        - WWDC session: *Add rich graphics to your SwiftUI app*

```swift
Button(action: undo) {
    Image(systemName: "arrow.uturn.backward")
}
.symbolVariant(.circle.fill)
.foregroundStyle(.white, .yellow, .red)  // primary, secondary, tertiary
// .foregroundStyle(.red, .secondary)
// .foregroundStyle(.red, .regularMaterial)
```

## Direct and reflect focus in SwiftUI

[Recording](https://wwdc.io/share/wwdc21/10023)

> With device input — as with all things in life — where you put focus matters.
> Discover how you can move focus in your app with SwiftUI, programmatically dismiss the keyboard, and build large navigation targets from small views.
> Together, these APIs can help you simplify your app’s interface and make it more powerful for people to find what they need.

- generally, SwiftUI handles focus automatically
    - new APIs allow for us to control focus
- control focus using the `@FocusState` property wrapper
    - state changes depending on state of focus
    - is an Enum that has to be hashable and optional
    - then use `.focused()` modifier to link the focus state to views
    - set `focusedField` to `nil` to not have anything in focus
        - automatically dismisses keyboard if it was available

```swift
enum Field: Hashable { case email, password }

struct ContentView: View {
    @FocusState private var focusedField: Field?
    var body: some View {
        VStack {
            TextField("Email", text: $email)
                .focused($focusedField, equals: .email)
            SecureField("Password", text: $password)
                .focused($focusedField, equals: .password)
        }
        .onSubmit {
            if !isEmailValid {
                focusedField = .email  // Move focus to the email input field.
            } else {
                focusedField = nil
            }
        }
    }
}
```

- can create focusable navigation targets, too
    - add the `.focusSection()` modifier to any view that contains a focusable view (e.g. button, text field)

```swift
HStack {
    VStack {
        TextField("Email", text: $email)
        SecureField("Password", text: $password)
        SignInWithAppleButton(...)
    }
    .focusSection()  // Required in order to navigate focus back from the other VStack.
    .onSubmit { ... }
    VStack {
        Image(photoName)
        BrowsePhotosButton()
    }
    .focusSection() // Allows this entire VStack to be focusable.
}
```

## SwiftUI on the Mac: Build the fundamentals

[Recording](https://wwdc.io/share/wwdc21/10062)

> Code along with us as we use SwiftUI to build a Mac app from start to finish.
> Discover four principles all great Mac apps have in common, and learn how to apply those principles in practice using SwiftUI.
> We’ll show you how to create a powerful, flexible sidebar experience and transform lists to tables within a detail view, then discuss best best practices for data organization.
> Next, we’ll explore the simple .searchable modifier and find out how to add support for the toolbar and search.
> And to close out part one, we’ll learn how to build a great multiple-window experience and provide menu bar support.
>
> This is the first session in a two-part Code-Along series.
> To get the most out of this series, we recommend that you have some basic familiarity with SwiftUI.
> For more background, watch "Introduction to SwiftUI" from WWDC20.

- this code-along demonstrates the following technology:
    - sidebar creation in a `NavigationView`
    - `@SceneStorage` property wrapper for storing state across opening and quitting an app
    - `DisclosureGroup` for optionally showing sub-menus
    - `.badge` modifier to `Label` views
    - `Table` views to show tabular data in columns and rows
        - how to add sorting options
    - add buttons to the toolbar
        - including a `DisplayMode` button (e.g. for table vs. list vs. collection of images, etc.)
    - `.searchable` modifier
    - adding commands to the menu and keyboard shortcuts
        - use `FocusedBinding` property wrapper to get bindings from currently used window

## SwiftUI on the Mac: The finishing touches

[Recording](https://wwdc.io/share/wwdc21/10289)

> Join us for part two of our Code-Along series as we use SwiftUI to build a Mac app from start to finish.
> The journey continues as we explore how our sample gardening app can adapt to a person’s preferences and specific workflows.
> Learn how SwiftUI apps can automatically react to system settings, and discover how you can use that information to add more personality to an app.
> We’ll show you how you can give people the flexibility to customize an app through Settings, and explore how to use different workflows for manipulating someone’s data (like drag and drop).
> To finish, we’ll show you how you can move data to and from an app, incorporating features like Continuity Camera to provide a simple workflow for importing images.
>
> This is the second session in a two-part Code-Along series.
> To get the most out of this session, we recommend first watching “SwiftUI on the Mac: Build the fundamentals.”
> And for more background on working with these frameworks, watch Introduction to SwiftUI from WWDC20.

- this code-along demonstrates the following technology:
    - how to add a custom AccentColor for the app
    - add and customize a `Settings` scene with a `TabView` with multiple children views
        - `@AppStorage` property wrapper to persist data using `UserDefaults`
        - including a `.tag()` modifier for the selection in a `Picker`
    - create a `Binding` with a `get` and `set` closure (demonstrated below)
    - enable drag-and-drop support in a `Table`
        - enable dragging a row by building the `Table` using a "row-builder" with the `row:` parameter instead of initializing the `Table` with a collection
        - enable drop by adding a `.onInsert` modifier to the `ForEach` in the row-builder closure
    - how to let the user export data
        - add the command to the Menu where the "Export..." command usually goes
        - open the file exporting menu and provide the data as a "document"
    - how to import data from another device (brief on details, but can look at source code for help)

```swift
@SceneStorage("selection") private var selectedGardenID: Garden.ID?
@AppStorage("defaultGarden") private var defaultGardenID: Garden.ID?

// Get the selected garden if available, else check if there is a default garden.
// On change, only set the scene's `selectedGardenID`, not the app's global default.
private var selection: Binding<Garden.ID?> {
    Binding(get: { seletedGardenID ?? defaultGardenID  }, set: { selectedGardenID = $0  })
}
```

## Craft search experiences in SwiftUI

[Recording](https://wwdc.io/share/wwdc21/10176)

> Discover how you can help people quickly find specific content within your apps.
> Learn how to use SwiftUI’s `.searchable` modifier in conjunction with other views to best incorporate search for your app.
> And we’ll show you how to elevate your implementation by providing search suggestions to help people understand the types of searches they can perform.

- can add a search bar to a view using the `.searchable` modifier
    - below is a basic example of adding a search bar to a `NavigationView`

```swift
NavigationView {
    WeatherList(text: $text) {
        ForEach(data) { item in
            WeatherCell(item)
        }
    }
}
.searchable(text: $text)
```

- for this example weather app, the view changes when a search is in progress
    - can adapt to this dynamically using the `.isSearching` environment variable

```swift
// inside `WeatherList`

@Binding var text: String

@Environment(\.isSearching) private var isSearching: Bool

var body: some View {
    WeatherCitiesList()
        .overlay {
            if isSearching && !text.isEmpty {
                WeatherSearchResults()
            }
        }
}
```

- can add suggestions for the search bar as a trailing closure to `.searchable`
    - use the `onSubmit` modifier for when to commit the search results

```swift
NavigationView {
    Sidebar()
    DetailView()
}
.searchable(text: $text) {
    ForEach(suggestions) { suggestion in
        Button {
            text = suggestion.text
        } label: {
            ColorsSuggestionLabel(suggestion)
        }
    }
}
.onSubmit(of: .search) {
    fetchSearchResults()
}
```
