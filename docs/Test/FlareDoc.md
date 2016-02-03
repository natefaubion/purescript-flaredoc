## Module Test.FlareDoc

#### `withPackage`

``` purescript
withPackage :: forall eff. String -> (Documentation -> Eff (ajax :: AJAX, console :: CONSOLE, chan :: Chan, dom :: DOM) Unit) -> Eff (ajax :: AJAX, err :: EXCEPTION, console :: CONSOLE, chan :: Chan, dom :: DOM) Unit
```

Parse a package description and run the interactive documentation.

#### `flareDoc'`

``` purescript
flareDoc' :: forall t e. (Interactive t) => ElementId -> Documentation -> String -> String -> t -> Eff (chan :: Chan, dom :: DOM | e) Unit
```

Like `flareDoc`, but the HTML element can be specified.

#### `flareDoc`

``` purescript
flareDoc :: forall t e. (Interactive t) => Documentation -> String -> String -> t -> Eff (chan :: Chan, dom :: DOM | e) Unit
```

Add an interactive documentation entry. The `String` arguments specify the
module name and the function name, respectively.


### Re-exported from Test.FlareCheck:

#### `ElementId`

``` purescript
type ElementId = String
```

#### `Flare`

``` purescript
data Flare a
```

A `Flare` is a `Signal` with a corresponding list of HTML elements
for the user interface components.

##### Instances
``` purescript
Functor Flare
Apply Flare
Applicative Flare
```

#### `Label`

``` purescript
type Label = String
```

#### `Renderable`

``` purescript
data Renderable
```

A data type that describes possible output actions and values for an
interactive test.

#### `UI`

``` purescript
newtype UI e a
```

The main data type for a Flare UI. It encapsulates the `Eff` action
which is to be run when setting up the input elements and corresponding
signals.

##### Instances
``` purescript
Functor (UI e)
Apply (UI e)
Applicative (UI e)
(Semigroup a) => Semigroup (UI e a)
(Monoid a) => Monoid (UI e a)
(Semiring a) => Semiring (UI e a)
(Ring a) => Ring (UI e a)
(ModuloSemiring a) => ModuloSemiring (UI e a)
(DivisionRing a) => DivisionRing (UI e a)
(Num a) => Num (UI e a)
(Bounded a) => Bounded (UI e a)
(BooleanAlgebra a) => BooleanAlgebra (UI e a)
```

#### `Flammable`

``` purescript
class Flammable a where
  spark :: forall e. UI e a
```

A type class for input parameters for interactive tests. Instances for
type `a` must provide a way to create a Flare UI which holds a value of
type `a`.

##### Instances
``` purescript
Flammable Number
Flammable Int
Flammable String
Flammable Char
Flammable Boolean
(Flammable a, Flammable b) => Flammable (Tuple a b)
(Flammable a) => Flammable (Maybe a)
(Flammable a, Flammable b) => Flammable (Either a b)
(Read a) => Flammable (Array a)
(Read a) => Flammable (List a)
```

#### `Interactive`

``` purescript
class Interactive t where
  interactive :: forall e. UI e t -> UI e Renderable
```

A type class for interactive tests. Instances must provide a way to create
a Flare UI which returns a `Renderable` output.

##### Instances
``` purescript
Interactive Number
Interactive Int
Interactive String
Interactive Char
Interactive Boolean
Interactive Ordering
Interactive GenericSpine
(Generic a) => Interactive (Maybe a)
(Generic a, Generic b) => Interactive (Either a b)
(Generic a, Generic b) => Interactive (Tuple a b)
(Generic a) => Interactive (Array a)
(Generic a) => Interactive (List a)
(Flammable a, Interactive b) => Interactive (a -> b)
```

#### `Read`

``` purescript
class Read a where
  typeName :: Proxy a -> String
  defaults :: Proxy a -> String
  read :: String -> Maybe a
```

A class for types which can be parsed from a `String`. This class is used
to construct input fields for `Array a` and `List a`.

##### Instances
``` purescript
Read Number
Read Int
Read String
Read Char
Read Boolean
```

#### `boolean`

``` purescript
boolean :: forall e. Label -> Boolean -> UI e Boolean
```

Creates a checkbox for a `Boolean` input from a given label and default
value.

#### `boolean_`

``` purescript
boolean_ :: forall e. Boolean -> UI e Boolean
```

Like `boolean`, but without a label.

#### `button`

``` purescript
button :: forall a e. Label -> a -> a -> UI e a
```

Creates a button which yields the first value in the default state and
the second value when it is pressed.

#### `buttons`

``` purescript
buttons :: forall a e. Array a -> (a -> String) -> UI e (Maybe a)
```

Create a button for each element of the array. The whole component
returns `Nothing` if none of the buttons is pressed and `Just x` if
the button corresponding to the element `x` is pressed.

#### `fieldset`

``` purescript
fieldset :: forall e a. Label -> UI e a -> UI e a
```

Group the components of a UI inside a fieldset element with a given title.

#### `flareCheck`

``` purescript
flareCheck :: forall t e. (Interactive t) => Label -> t -> Eff (chan :: Chan, dom :: DOM | e) Unit
```

Run an interactive test. The label provides a title for the test.

#### `flareCheck'`

``` purescript
flareCheck' :: forall t e. (Interactive t) => ElementId -> Label -> t -> Eff (chan :: Chan, dom :: DOM | e) Unit
```

Run an interactive test. The ID specifies the parent element to which
the test will be appended and the label provides a title for the test.

#### `foldp`

``` purescript
foldp :: forall a b e. (a -> b -> b) -> b -> UI e a -> UI e b
```

Create a past dependent component. The fold-function takes the current
value of the component and the previous value of the output to produce
the new value of the output.

#### `int`

``` purescript
int :: forall e. Label -> Int -> UI e Int
```

Creates an input field for an `Int` from a given label and default
value. The returned value is guaranteed to be within the allowed integer
range.

#### `intRange`

``` purescript
intRange :: forall e. Label -> Int -> Int -> Int -> UI e Int
```

Creates an input field for an `Int` from a given label, minimum and
maximum values as well as a default value. The returned value is
guaranteed to be within the given range.

#### `intRange_`

``` purescript
intRange_ :: forall e. Int -> Int -> Int -> UI e Int
```

Like `intRange`, but without a label.

#### `intSlider`

``` purescript
intSlider :: forall e. Label -> Int -> Int -> Int -> UI e Int
```

Creates a slider for an `Int` input from a given label, minimum and
maximum values as well as a default value.

#### `intSlider_`

``` purescript
intSlider_ :: forall e. Int -> Int -> Int -> UI e Int
```

Like `intSlider`, but without a label.

#### `int_`

``` purescript
int_ :: forall e. Int -> UI e Int
```

Like `int`, but without a label.

#### `interactiveFoldable`

``` purescript
interactiveFoldable :: forall f a e. (Foldable f, Generic a) => UI e (f a) -> UI e Renderable
```

A default `interactive` implementation for `Foldable` types.

#### `interactiveGeneric`

``` purescript
interactiveGeneric :: forall a e. (Generic a) => UI e a -> UI e Renderable
```

A default `interactive` implementation for types with a `Generic` instance.

#### `interactiveShow`

``` purescript
interactiveShow :: forall t e. (Show t) => UI e t -> UI e Renderable
```

A default `interactive` implementation for any `Show`able type.

#### `lift`

``` purescript
lift :: forall e a. Eff (chan :: Chan, dom :: DOM | e) (Signal a) -> UI e a
```

Lift a `Signal` inside the `Eff` monad to a `UI` component.

#### `liftSF`

``` purescript
liftSF :: forall e a b. (Signal a -> Signal b) -> UI e a -> UI e b
```

Lift a function from `Signal a` to `Signal b` to a function from
`UI e a` to `UI e b` without affecting the components. For example:

``` purescript
dropRepeats :: forall e a. (Eq a) => UI e a -> UI e a
dropRepeats = liftSF S.dropRepeats
```

#### `number`

``` purescript
number :: forall e. Label -> Number -> UI e Number
```

Creates an input field for a `Number` from a given label and default
value.

#### `numberRange`

``` purescript
numberRange :: forall e. Label -> Number -> Number -> Number -> Number -> UI e Number
```

Creates an input field for a `Number` from a given label,
minimum value, maximum value, step size as well as default value.
The returned value is guaranteed to be within the given range.

#### `numberRange_`

``` purescript
numberRange_ :: forall e. Number -> Number -> Number -> Number -> UI e Number
```

Like `numberRange`, but without a label.

#### `numberSlider`

``` purescript
numberSlider :: forall e. Label -> Number -> Number -> Number -> Number -> UI e Number
```

Creates a slider for a `Number` input from a given label,
minimum value, maximum value, step size as well as default value.

#### `numberSlider_`

``` purescript
numberSlider_ :: forall e. Number -> Number -> Number -> Number -> UI e Number
```

Like `numberSlider`, but without a label.

#### `number_`

``` purescript
number_ :: forall e. Number -> UI e Number
```

Like `number`, but without a label.

#### `optional`

``` purescript
optional :: forall a e. Label -> Boolean -> a -> UI e (Maybe a)
```

Creates a checkbox that returns `Just x` if enabled and `Nothing` if
disabled. Takes a label, the initial state (enabled or disabled) and
the default value `x`.

#### `optional_`

``` purescript
optional_ :: forall a e. Boolean -> a -> UI e (Maybe a)
```

Like `optional`, but without a label.

#### `radioGroup`

``` purescript
radioGroup :: forall e a. Label -> a -> Array a -> (a -> String) -> UI e a
```

Creates a group of radio buttons to choose from a list of options. The
first option is selected by default. The rest of the options is given as
an array.

#### `radioGroup_`

``` purescript
radioGroup_ :: forall e a. a -> Array a -> (a -> String) -> UI e a
```

Like `radioGroup`, but without a label.

#### `runFlare`

``` purescript
runFlare :: forall e. ElementId -> ElementId -> UI e String -> Eff (dom :: DOM, chan :: Chan | e) Unit
```

Renders a Flare UI to the DOM and sets up all event handlers. The two IDs
specify the DOM elements to which the controls and the output will be
attached, respectively.

#### `runFlareShow`

``` purescript
runFlareShow :: forall e a. (Show a) => ElementId -> ElementId -> UI e a -> Eff (dom :: DOM, chan :: Chan | e) Unit
```

Like `runFlare` but uses `show` to convert the contained value to a
`String` before rendering to the DOM (useful for testing).

#### `runFlareWith`

``` purescript
runFlareWith :: forall e a. ElementId -> (a -> Eff (dom :: DOM, chan :: Chan | e) Unit) -> UI e a -> Eff (dom :: DOM, chan :: Chan | e) Unit
```

Renders a Flare UI to the DOM and sets up all event handlers. The ID
specifies the HTML element to which the controls are attached. The
function argument will be mapped over the `Signal` inside the `Flare`.

#### `select`

``` purescript
select :: forall e a. Label -> a -> Array a -> (a -> String) -> UI e a
```

Creates a select box to choose from a list of options. The first option
is selected by default. The rest of the options is given as an array.

#### `select_`

``` purescript
select_ :: forall e a. a -> Array a -> (a -> String) -> UI e a
```

Like `select`, but without a label.

#### `setupFlare`

``` purescript
setupFlare :: forall e a. UI e a -> Eff (chan :: Chan, dom :: DOM | e) { components :: Array Element, signal :: Signal a }
```

Low level function to get direct access to the HTML elements and the
`Signal` inside a Flare UI.

#### `string`

``` purescript
string :: forall e. Label -> String -> UI e String
```

Creates a text field for a `String` input from a given label and default
value.

#### `stringPattern`

``` purescript
stringPattern :: forall e. Label -> String -> String -> UI e String
```

Creates a text field for a `String` input from a given label, validation
pattern (HTML5 `pattern` attribute), and a default value.

#### `stringPattern_`

``` purescript
stringPattern_ :: forall e. String -> String -> UI e String
```

Like `stringPattern`, but without a label.

#### `string_`

``` purescript
string_ :: forall e. String -> UI e String
```

Like `string`, but without a label.

#### `wrap`

``` purescript
wrap :: forall e a. Signal a -> UI e a
```

Encapsulate a `Signal` within a `UI` component.

#### `(<**>)`

``` purescript
(<**>) :: forall a b e. UI e a -> UI e (a -> b) -> UI e b
```

_left-associative / precedence 4_

A flipped version of `<*>` for `UI` that arranges the components in the
order of appearance.

