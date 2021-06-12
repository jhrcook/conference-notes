# WWDC 2021 Notes

### Watch List

- [x] What‘s new in Swift
- [x] Meet DocC documentation in Xcode
- [x] Meet async/await in Swift
- [ ] Protect mutable state with Swift actors
- [ ] Explore structured concurrency in Swift
- [ ] Meet AsyncSequence
- [ ] Use async/await with URLSession
- [x] What's new in SwiftUI
- [ ] Demystify SwiftUI
- [ ] Add rich graphics to your SwiftUI app
- [ ] Direct and reflect focus in SwiftUI
- [ ] Discover concurrency in SwiftUI
- [ ] Swift concurrency: Update a sample app
- [ ] Use the camera for keyboard input in your app
- [ ] Ultimate application performance survival guide
- [ ] Meet the Screen Time API
- [x] What’s new in SF Symbols

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
