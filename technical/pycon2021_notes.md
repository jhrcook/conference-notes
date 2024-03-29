```python
!pip install pandera hypothesis black
```

    Requirement already satisfied: pandera in /usr/local/lib/python3.7/dist-packages (0.6.4)
    Requirement already satisfied: hypothesis in /usr/local/lib/python3.7/dist-packages (6.14.0)
    Requirement already satisfied: black in /usr/local/lib/python3.7/dist-packages (21.6b0)
    Requirement already satisfied: wrapt in /usr/local/lib/python3.7/dist-packages (from pandera) (1.12.1)
    Requirement already satisfied: typing-inspect>=0.6.0 in /usr/local/lib/python3.7/dist-packages (from pandera) (0.7.1)
    Requirement already satisfied: numpy>=1.9.0 in /usr/local/lib/python3.7/dist-packages (from pandera) (1.19.5)
    Requirement already satisfied: typing-extensions>=3.7.4.3; python_version < "3.8" in /usr/local/lib/python3.7/dist-packages (from pandera) (3.7.4.3)
    Requirement already satisfied: pandas>=0.25.3 in /usr/local/lib/python3.7/dist-packages (from pandera) (1.1.5)
    Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.7/dist-packages (from pandera) (20.9)
    Requirement already satisfied: attrs>=19.2.0 in /usr/local/lib/python3.7/dist-packages (from hypothesis) (21.2.0)
    Requirement already satisfied: sortedcontainers<3.0.0,>=2.1.0 in /usr/local/lib/python3.7/dist-packages (from hypothesis) (2.4.0)
    Requirement already satisfied: appdirs in /usr/local/lib/python3.7/dist-packages (from black) (1.4.4)
    Requirement already satisfied: typed-ast>=1.4.2; python_version < "3.8" in /usr/local/lib/python3.7/dist-packages (from black) (1.4.3)
    Requirement already satisfied: regex>=2020.1.8 in /usr/local/lib/python3.7/dist-packages (from black) (2021.4.4)
    Requirement already satisfied: pathspec<1,>=0.8.1 in /usr/local/lib/python3.7/dist-packages (from black) (0.8.1)
    Requirement already satisfied: mypy-extensions>=0.4.3 in /usr/local/lib/python3.7/dist-packages (from black) (0.4.3)
    Requirement already satisfied: toml>=0.10.1 in /usr/local/lib/python3.7/dist-packages (from black) (0.10.2)
    Requirement already satisfied: click>=7.1.2 in /usr/local/lib/python3.7/dist-packages (from black) (7.1.2)
    Requirement already satisfied: python-dateutil>=2.7.3 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.25.3->pandera) (2.8.1)
    Requirement already satisfied: pytz>=2017.2 in /usr/local/lib/python3.7/dist-packages (from pandas>=0.25.3->pandera) (2018.9)
    Requirement already satisfied: pyparsing>=2.0.2 in /usr/local/lib/python3.7/dist-packages (from packaging>=20.0->pandera) (2.4.7)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.7/dist-packages (from python-dateutil>=2.7.3->pandas>=0.25.3->pandera) (1.15.0)

# PyCon 2021 Notes

[Website](https://us.pycon.org/2021/) | [Talks Schedule](https://us.pycon.org/2021/schedule/talks/) | [YouTube page](https://www.youtube.com/c/PyConUS/videos) | [PyCon 2021 Playlist](https://www.youtube.com/watch?v=z_hm5oX7ZlE&list=PL2Uw4_HvXqvYk1Y5P8kryoyd83L_0Uk5K)

### Talk List

- [x] Keynote - Imaging Rembrandt's The Night Watch at 5 µm Resolution with Python
- [x] Reproducible and maintainable data science code with Kedro
- [x] What we learned from Papermill to operationalize notebooks
- [x] Testing stochastic AI models with Hypothesis
- [x] From NumPy to PyTorch, A Story of API Compatibility
- [x] PyTesting the Limits of Machine Learning
- [x] When is an exception not an exception? Using warnings in Python
- [x] More Fun With Hardware and CircuitPython - IoT, Wearables, and more!
- [x] Protocol: the keystone of type hints
- [ ] Static Sites with Sphinx and Markdown
- [x] The Road to Pattern Matching in Python
- [x] Using Declarative Configs for Maintainable Reproducible Code
- [x] Getting an Edge with Network Analysis with Python
- [ ] Python Performance at Scale - Making Python Faster at Instagram
- [ ] No, Maybe and Close Enough: Using Probabilistic Data Structures in Python
- [x] The magic of "self": How Python inserts "self" into methods
- [x] What are quantum computers, and how can we train them in Python?
- [x] Statistical Typing: A Runtime Typing System for Data Science and Machine Learning
- [x] Unexpected Execution: Wild Ways Code Execution can Occur in Python
- [x] Large Scale Data Validation (with Spark and Dask)
- [x] Patterns of ML Models in Production

## Keynote - Imaging Rembrandt's *The Night Watch* at 5 µm Resolution with Python

⭐⭐⭐⭐⭐

**Robert Erdmann** ([video](https://youtu.be/z_hm5oX7ZlE))

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/The_Night_Watch_-_HD.jpg/760px-The_Night_Watch_-_HD.jpg" width=500 alt="The Night Watch">

- an in-depth description of how Python and many of the common data science libraries (e.g. Jupyter, scikit-learn, pandas, numpy) to capture a high resolution image of this 12 x 14 foot painting
- there are 4 main systems:
    - *Imaging Frame Subsystem*: mechanical system for positioning the camera
    - *Imaging Subsystem*: image capturing and quality assessment
    - *Client Laptop*: a series of tools for remotely monitoring and controlling the imaging process
    - *Central Control Subsytem*: central hub for integrating the other subsystems
- some interesting notes:
    - they used a Hasselblad image, but cannot control the camera through a built-in API; thus they used OpenCV to screen-read Hasselblad's GUI to interact with the camera automatically
    - used 5 laser range-finders to measure the distance between the camera and the canvas in each image
    - used "shape from focus" to find the perfect depth for the camera at specific focus parameter settings
    - each photo is 600 MB on disk
- the final image is 925,000 px wide x 775,000 px tall with 5 µm resolution

### Conclusions

This was an amazing talk that blew me away.
It genuinely made me want to learn about image processing and I started thinking about smaller-scale image capture systems I could try to rig with a Raspberry Pi.
I highly recommend watching this Keynote presentation.

## Reproducible and maintainable data science code with [Kedro](https://github.com/quantumblacklabs/kedro)

⭐⭐⭐

**Yetunde Dada** ([video](https://youtu.be/JLTYNPoK7nw))

From the 'kerdo' GitHub repository:

| Feature | What is this? |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Project Template | A standard, modifiable and easy-to-use project template based on [Cookiecutter Data Science](https://github.com/drivendata/cookiecutter-data-science/). |
| Data Catalog | A series of lightweight data connectors used to save and load data across many different file formats and file systems, including local and network file systems, cloud object stores, and HDFS. The Data Catalog also includes data and model versioning for file-based systems. |
| Pipeline Abstraction | Automatic resolution of dependencies between pure Python functions and data pipeline visualisation using [Kedro-Viz](https://github.com/quantumblacklabs/kedro-viz). |
| Coding Standards | Test-driven development using [`pytest`](https://github.com/pytest-dev/pytest), produce well-documented code using [Sphinx](http://www.sphinx-doc.org/en/master/), create linted code with support for [`flake8`](https://github.com/PyCQA/flake8), [`isort`](https://github.com/PyCQA/isort) and [`black`](https://github.com/psf/black) and make use of the standard Python logging library. |
| Flexible Deployment | Deployment strategies that include single or distributed-machine deployment as well as additional support for deploying on Argo, Prefect, Kubeflow, AWS Batch and Databricks. |

### Conclusions

I think 'kedro' is an interesting framework for organizing and "productionizing" an ML project.
In my opinion, with good software-writing fundamentals, many of the problems that 'kedro' addresses are alreay solved and, in that case, the framework itself becomes another thing to learn and maintain.
That said, I know that many data scientists are not traditoinally-trained software engineers and have little interest in cultivating that skill.
In that scenario, I could see 'kedro' be a very useful tool.

## What we learned from Papermill to operationalize notebooks

⭐⭐

Alan Yu & Vasu Bhog ([video](https://youtu.be/pvaIi0l1GME))

- main problems they found: **interactivity, accessiblity, and maintainablity**
    - solved with Jupyter notebooks, but requires parameterization
- add Papermill-esque features to Azure Data Studio (Micosoft)
    - in can download a notebook from the internet (e.g. GitHub) and parameterize it with additions to the URL as queries
    - in a parameterized notebook, there is a "Run with parameters" button that opens a menu to input new parameter values (interactive re-parameterization)
    - global parameters in Jupyter books

### Conclusions

I do not currently use Azure tools, but this was a good example of the some of the variety and power.

## Testing stochastic AI models with Hypothesis

⭐⭐⭐⭐

**Marina Shvartz** ([video](https://youtu.be/uVjgkqEpgkE))

- problems with example-based testing:
    - up to the devleoper to create exhaustive tests
    - need to cover strange edge cases
    - time consuming
- property baesed testing:
    - define possible inputs
    - define the properties of the outputs
    - generate random examples
    - tests that the expected properties are met
- examples of different properties (demonstrated in the talk):
    - *commutivity*: order of inputs should not matter
    - *invariant* functions: properties that should not be changed by a function
    - *the oracle tests*: testing against alternative implementations (before and after refactorings)
- metamorphic testing is an approach to solve issues with "oracle tests" when there is nothing to compare to
    - apply a transformation to an input and make claims on the relationship of the ouput from the function being tested based on the expected behavior ("metamorphic relation")
- overview of ['hypothesis'](https://hypothesis.readthedocs.io):
    - key object is a "strategy": a recipe for generating data
    - there are some predefined strategies and custom ones can be written, too

```python
from hypothesis import given
from hypothesis import strategies as st


@given(
    st.floats(allow_infinity=False, allow_nan=False),
    st.floats(allow_infinity=False, allow_nan=False),
)
def test_given_floats_add_is_commutative(x: float, y: float):
    assert x + y == y + x
```

- there is built-in support for data science libraries (e.g. numpy, pandas)

```python
import numpy as np
from hypothesis.extra.numpy import array_shapes, arrays


@given(
    arrays(int, st.shared(array_shapes(min_dims=3, max_dims=3), key="shape")),
    arrays(int, st.shared(array_shapes(min_dims=3, max_dims=3), key="shape")),
)
def test_given_arrays_multiply_is_commutative(arr1: np.ndarray, arr2: np.ndarray):
    np.array_equal(arr1 * arr2, arr2 * arr1)
```

- defining custom strategies:
    - has to create some sort of callable such as a function or class
- hypothesis has several debugging tools
    - can have a strategory create examples
    - there is a `note()` functions to print additional information on failure
- if hypothesis causes a test to fail, it will automatically retain the input that caused the failure to be tested again in the future

### Conlcusions

This was a great introduction to the both the theory and practice of property-based testing.
I will definitely be looking more into the 'hypothesis' documentation and implement this form of testing into my own projects.

## From NumPy to PyTorch, A Story of API Compatibility

⭐⭐⭐

**Randall Hunt & Mike Ruberry** ([video](https://youtu.be/5wk13yle5GA))

- brief intro to ['numpy'](https://numpy.org)
    - Python library for operating on tensors (a.k.a arrays)
    - has two classes of functions: *composites* operate on multiple tensors and are writtin in Python; *primitives* operate directly on a tensor's values and are written in C++ (with bindings to Python)
- [PyTorch]() - hardware accelerators, autograd, and computational graphs
    - very similar API to numpy and numpy arrays and PyTorch tensors can be converted back and forth
    - PyTorch's operations can use hardware accelerators (GPUs)
    - PyTorch has built-in autogradient computation (example below)
    - can make computational graphs

```python
import torch

a = torch.tensor((1.0, 2.0), requires_grad=True)
b = torch.tensor((3.0, 4.0))
result = (a * b).sum()
result.backward()
a.grad
```

    tensor([3., 4.])

- steps for porting a numpy operator to PyTorch
    1. write a C++ implementation
    2. create an autograd formula
    3. comprehensive testing
        - PyTorch has a custom testing framework for automatically constructing tests that would otherwise be hundreds of lines long and take a lot of time to write manually

### Conclusions

Most of this talk was irrelevant to my day-to-day work, but it was fun to get a brief under-the-hood look at numpy and PyTorch.

## PyTesting the Limits of Machine Learning

⭐⭐

**Rebecca Bilbro, Daniel Sollis, & Patrick Deziel** ([video](https://youtu.be/GycRK_K0x2s))

- it is important to test all code that eventually becomes a product
    - I would include a research project in this cateogory, too

### Conclusions

I agree entirely with the presenters that it is esssential to test machine learning and, more broadly, data modeling.
Unfortunately, I do not think presentation addressed the main difficulties of testing these code bases.

*(The following is my opinion. It is possible that my data analysis workflow and usecases are substantially different from those of the presentors, in which case, my statements are not applicable.)*

The first difficulty is that a lot of the code written by data scientists is not made to be tested - thus, the first issue has nothing to do with writing tests, but, instead, adhering to core software tenants such as separability and D.R.Y.
By decomposing complex notebooks and scripts into submodules of functions, the individual units of a project can be easily tested like any other code.

The second difficulty is with testing the model itself.
This component was discussed by the presenters, but I think they missed a common issue: these models can take a long time to train/fit.
The demonstrations presented in the talk showed the tests fitting a model and assessing its performance on data.
In some cases, this can take several minutes to hours per model, leading to a test suite that consumes a substantial amount of time to run.

Finally, in the talk, the tests in the demonstration were focussed on testing how the model behaved (e.g. accuracy, F1 scores, etc.).
For me, this is a separate concern that can be addressed as part of an analysis in a project.
In this analysis, the researcher or data scientist compares the different aspects of the models and identifies their individual strengths and weaknesses.
I think the tests should be about the behavior of the code and the utility of a model is a research question.
For example, the tests are there to ensure that the code that pre-processes the data, fits models, etc. work as expected, not to fit the full model.

## More Fun With Hardware and CircuitPython - IoT, Wearables, and more!

⭐

**Nina Zakharenko** ([video](https://youtu.be/GnteZjiHVdA)

> Learn about programming hardware with Python and advanced uses of CircuitPython by walking through exciting demos of real-world projects in action.
> Advanced components like buttons, sensors, and screens bump up the fun and the interactivity of your project. > Level-up your hardware skills in this fast-paced talk!
>
> CircuitPython is the education-friendly fork of MicroPython that's been steadily rising in popularity as new releases increase stability, reliability, and speed.
> CircuitPython allows Python enthusiasts to quickly learn about hardware projects without having to learn something completely brand new.
> Given the rise in popularity, the Python community is quickly becoming familiar with the basics of CircuitPython.
> In fact, all attendees of PyCon US in 2019 were given a CircuitPython-compatible CircuitPlayground Express device with LEDs, speakers, sensors, and more, all usable without the need of learning to solder.
>
> If you're interested in doing more with hardware, this talk will point you in the right direction of where to go next. We'll start with choosing the right device for the scope of your project.
> Next, we'll scratch the surface of working with electronics -- what's a circuit? What are good resources for learning to solder?
> Lastly, I'll cover topics such as IoT, wearables, and adding interactivity to your projects with sensors, buttons, and switches with live demos of real-world projects I've created, along with sharing the build process and code for each.

This talk was really just an introduction to what can be done with CircuitPython and microcontrollers/electronics. Not much to do with how to use CircuitPython.

## When is an exception not an exception? Using warnings in Python

⭐⭐⭐⭐

**Reuven M. Lerner** ([recording](https://youtu.be/X0AjcpicNOM))

> If your code encounters a big problem, then you probably want to raise an exception.
> But what should your code do if it finds a small problem, one that shouldn't be ignored, but that doesn't merit an exception? Python's answer to this question is warnings.
>
> In this talk, I'll introduce Python's warnings, close cousins to exceptions but still distinct from them.
> We'll see how you can generate warnings, and what happens when you do. But then we'll dig deeper, looking at how you can filter and redirect warnings, telling Python which types of warnings you want to see, and which you want to hide.
> We'll also see how you can get truly fancy, turning some warnings into (potentially fatal) exceptions and handling certain types with custom callback functions.
>
> After this talk, you'll be able to take advantage of Python's warning system, letting your users know when something is wrong without having to choose between "print" and a full-blown exception.

- requirements for a good warning:
    1. non-fatal when running the program
    2. annoying, persistent to get user to change behaviour
    3. must tell user that if they don't change, bad things will happen
    4. must be used with enough time for the user to adjust
- exceptions:
    - good:
        - think of as a separate communication channel
        - can distinguish between them
        - can choose to ignore them or not
    - bad:
        - if not caught, will cause the program to end (effectively a crash)
- print messages:
    - is not forceful enough
    - cannot be trapped, filtered, inspected, etc.

```python
import warnings
warnings.warn("My warning about something.")
#> filename.py:3: UserWarning: My warning about something.
```

- output from warnings go to stderr (not stdout)
- different types of common warnings:
    - `UserWarning`
    - `DeprecationWarning`
    - `SyntaxWarning`
    - `RuntimeWarning`

```python
import warnings
warnings.warn("Here is a deprecation notice.", DeprecationWarning)
```

- advised to create custom warning (and exception) classes
    - allows for better handling and filtering

```python
import warnings

class ArgsChangingWarning(UserWarning):
    pass

warnings.warn("Arguments will change soon.", ArgsChangingWarning)
```

- can decide what should happen to a warning based on
    - type of warning
    - message in the warning
    - module the warning was in
- by default, a warning is only printed once per file
- the "stacklevel" of the warning determines which part of the stack the warning was raised in is presented to the user
    - the default is `stacklevel=1`, which is just the `warnings.warn()` function

```python
def hello(name: str) -> str:
    warnings.warn("Arguments will change soon.", ArgsChangingWarning, 2)
    return f"Hello, {name}!"


print(hello("Josh"))
#> userhello.py:5: ArgsChangingWarning: Arguments will change soon.
#>    print(hello("Josh"))
#> Hello, Josh!
```

- six different warning filtering actions:
    1. "default": print a warnings once per combination of module and line number
    2. "error": raise an exception (because warnings are actually a sublcass of exceptions in Python)
    3. "ignore": ignore the warning
    4. "always": always print the warning
    5. "module": print once per module (regardless of line number)
    6. "once": only once no matter what
- can change behavor on CLI call with the `-W` switch
    - other filtering options with the `-W`, too
    - do not work for custom warnings in code

```bash
python3 -Walways ./usehello.py
```

- filtering wanrings in our code
    - `warnings.filterwarngings()`: specify all five filter elements (type, module, regular expression, etc.)
        - usually more than we really need
    - `warnings.simplefilter()`: specify the action, category, and line number
        - usually the one we'll want to use
    - `warnings.resetwarnings()`: resent these settings

```python
import warnings
from hello import hello, ArgsChangingWarning

# Set the default bahvior for our custom warning.
warnings.simplefilter("default", ArgsChangingWarning)

print(hello("Josh"))
print(hello("world"))
```

- context manager for catching warnings

```python
with warnings.catch_warnings():
    warnings.simplefilter("ignore")
    poorly_behaved_function()
```

- where to put thse settings in code:
    - can change settings at the top of the module so it gets run when the module is imported

## Protocol: the keystone of type hints

⭐⭐⭐⭐

**Luciano Ramalho** ([recording](https://youtu.be/kDDCKwP7QgQ))

> The static type system supporting type hints in Python is becoming more expressive with each new PEP, but PEP 544 – Protocols: Structural subtyping (static duck typing) is the most important enhancement since type hints were first introduced.
> The `typing.Protocol` special class lets you define types in terms of the interface implemented by objects, regardless of type hierarchies, in the spirit of duck typing – but in a way that can be verified by static type checkers and IDEs.
>
> Without `typing.Protocol`, it was impossible to correctly annotate many APIs considered Pythonic, including many functions in the standard library itself.
> In this talk you will learn the concepts and benefits of static duck typing, through actual examples of increasing complexity taken from type hints of standard library functions in the official typeshed project.

- the `python/typeshed` code repository contains the code hints for functions in the python standard library
- *duck typing*: only check that an object has the subset of abilities that are needed by a chunk of code (e.g. a function)
    - *static duck typing*: static type checking of structural objects
        - this is the actual term used in PEP 544
- the standard `max()` function supports many different types of objects

```python
max(3.14, 2.17)
```

    3.14

```python
max(5, 8)
```

    8

```python
from fractions import Fraction

max(Fraction(5, 8), Fraction(7, 12))
```

    Fraction(5, 8)

```python
max("beta", "alpha")
```

    'beta'

```python
max([10, 11, 12], [10, 12])
```

    [10, 12]

```python
max({10, 11, 12}, {10, 12})
```

    {10, 11, 12}

- let's make a `mymax()` function that takes two floats and returns the maximum value as a float

```python
def mymax(a: float, b: float) -> float:
    if a >= b:
        return a
    return b

mymax(3.14, 2.17)
#> 3.14
```

- this function works, but mypy would throw an error if I tried to use anything but floats (such as shown below)

```python
from fraction import Fraction

mymax(Fraction(5, 8), Fraction(7, 12))  # mypy error
#> Fraction(5, 8)
```

- one possible solution is to use the `Number` ABC from the Python standard library, but this does not solve the problem because it does not define a `>=` operation

```python
from numbers import Number

# Does not solve the issue raised by mypy.
def mymax(a: Number, b: Number) -> Number:
    if a >= b:
        return a
    return b
```

- another option could be to use a numeric `Union` type
    - but now this returns a custom type that I have to deal with
    - also, if using `Fraction`, would get errors from mypy when trying to access attributes (e.g. `my_frac.denominator`) because the custom type does not have that attribute

```python
from decimal import Decimal
from fractions import Fraction
from typing import Union

Numeric = Union[float, Decimal, Fraction]

# Satisfies mypy's checks.
def mymax(a: Numeric, b: Numeric) -> Numeric:
    if a >= b:
        return a
    return b

# In my test suite...
@pytest.mark.parameterize("a, b, expected", [(1.1, 2.2, 2,2)]
def test_two_floats(a: float, b: float, expected: float) -> None:
    result: float = my.max(a, b)  # mypy: incompatible types
    assert result == expected
```

- a better solution would be to use a *restricted* type built with `TypeVar`
    - simillar in use to a generic type
    - this is a good solution if you only want to support numeric types, but cannot extend to any type that implements `>=` such as lists and sets

```python
from decimal import Decimal
from fractions import Fraction
from typing import TypeVar

NumberT = TypeVar("NumberT", float, Decimal, Fraction)

# Satisfies mypy's checks.
def mymax(a: NumberT, b: NumberT) -> Numeric:
    if a >= b:
        return a
    return b
```

- could use a protocol
    - but with this implementation, we run into a similar problem as when using the `Uinon` type: the return type is not the expected type

```python
from typing import TypeVar, Protocol, Any

class SupportsLessThan(Protocol):
    def __lt__(self, other: Any) -> bool:
        ...

def mymax(a: SupportsLessThan, b: SupportsLessThan) -> SupportsLessThan:
    if b < a:  # refactored to use `<` instead of `>=`
        return a
    return b
```

- best solution is to use a `Protocol` with a *bounded* `TypeVar`
    - note that the previous use of `TypeVar` implemented a *restricted* `TypeVar`

```python
from typing import TypeVar, Protocol, Any

class SupportsLessThan(Protocol):
    def __lt__(self, other: Any) -> bool: ...


LessT = TypeVar("LessT", bound=SupportsLessThan)

def mymax(a: LessT, b: LessT) -> LessT:
    if b < a:
        return a
    return b
```

## Statistical Typing: A Runtime Typing System for Data Science and Machine Learning

⭐⭐⭐⭐

**Niels Bantilan** ([recording](https://youtu.be/PI5UmKi14cM))

> Data science and machine learning rely on high quality datasets for visualization, statistical inference, and modeling.
> However, the barriers to testing data processing, analysis, or model-training code are high, even with the extensive tooling that the python ecosystem offers, such as pandas, pytest, and hypothesis.
>
> To address this problem, in this talk I define statistical typing as a general concept describing a runtime typing system, which extends primitive data types like bool, str, and float into the class of statistical data types.
> By providing additional semantics about the properties held by a collection of data points, statistical typing enables us to naturally express types as multivariate schemas.
> It also enables us to implement schemas as generative data contracts, which serve to both validate data at runtime and generate valid samples for testing purposes.
>
> I'll use ['pandera'](https://pandera.readthedocs.io/en/stable/), a pandas data testing library, to illustrate how statistical typing makes data testing easier by enabling you to validate real-world data with reusable schemas and isolate units of processing, analysis, and model-training code.

- statistical type specifications using schemas
    - need to include:
        1. **primitive datatype**: `int`, `float`, `bool`, ...
        2. **deterministic properties**: e.g. domain of possible values such as `x >= 0`
        3. **probabilistic properties**: values to describe the distribution of values (e.g. mean, s.d.)
- ['pandera'](https://pandera.readthedocs.io/en/stable/): library for statistical typing
    - below is an example of validating a data frame of housing prices

```python
import pandera as pa
from pandera import typing as pat

PROPERTY_TYPES = ["condo", "townhouse", "house"]


class BaseSchema(pa.SchemaModel):
    square_footage: pat.Series[int] = pa.Field(
        in_range={"min_value": 0, "max_value": 3000}
    )
    n_bedrooms: pat.Series[int] = pa.Field(in_range={"min_value": 0, "max_value": 10})
    prive: pat.Series[int] = pa.Field(in_range={"min_value": 0, "max_value": 1000000})

    class Config:
        coerce = True


class RawData(BaseSchema):
    property_type: pat.Series[str] = pa.Field(isin=PROPERTY_TYPES)


class ProcessedData(BaseSchema):
    property_type_condo: pat.Series[int] = pa.Field(isin=[0, 1])
    property_type_townhouse: pat.Series[int] = pa.Field(isin=[0, 1])
    property_type_house: pat.Series[int] = pa.Field(isin=[0, 1])


# Validate input and output types of functions.


@pa.check_types
def process_data(raw_data: pat.DataFrame[RawData]) -> pat.DataFrame[ProcessedData]:
    ...


@pa.check_types
def train_model(processed_data: pat.DataFrame[ProcessedData]):
    ...
```

- can bootstrap a schema from an existing data frame
    - exports as a YAML file or Python script

```python
import seaborn as sns

iris = sns.load_dataset("iris")[["sepal_length", "species"]]
iris.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal_length</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>

```python
schema = pa.infer_schema(iris)
print(schema.to_script())  # or `schema.to_yaml()`
```

    from pandera import (
        DataFrameSchema,
        Column,
        Check,
        Index,
        MultiIndex,
        PandasDtype,
    )

    schema = DataFrameSchema(
        columns={
            "sepal_length": Column(
                pandas_dtype=PandasDtype.Float64,
                checks=[
                    Check.greater_than_or_equal_to(min_value=4.3),
                    Check.less_than_or_equal_to(max_value=7.9),
                ],
                nullable=False,
                allow_duplicates=True,
                coerce=False,
                required=True,
                regex=False,
            ),
            "species": Column(
                pandas_dtype=PandasDtype.String,
                checks=None,
                nullable=False,
                allow_duplicates=True,
                coerce=False,
                required=True,
                regex=False,
            ),
        },
        index=Index(
            pandas_dtype=PandasDtype.Int64,
            checks=[
                Check.greater_than_or_equal_to(min_value=0.0),
                Check.less_than_or_equal_to(max_value=149.0),
            ],
            nullable=False,
            coerce=False,
            name=None,
        ),
        coerce=True,
        strict=False,
        name=None,
    )

## The magic of "self": How Python inserts "self" into methods

⭐⭐⭐⭐

**Sebastiaan Zeeff** ([recording](https://youtu.be/ANLjBsWHshc))

> A phrase that I hear a lot is "Python is easy to learn, but hard to master".
> In a way that's true: Python is easy to learn because its high level of abstraction allows you to focus on the business logic of what you're trying to do instead of the lower-level implementation details.
>
> At the same time, Python's abstraction isn't magical: Its versatile data model allows you to hook into almost every part of the language to implement objects that behave just as Python's built-in objects do, enabling you to create similarly high-leveled interfaces for your own objects.
> That's where "hard to master" comes in: There is so much to learn that you're never done learning.
>
> In this talk, I want to entice you to look beyond Python's high-level interface into the wonderful landscape of its data model.
> I'll do that by explaining one of Python's most "magical" features: The automatic insertion of self into methods. Often, to beginners, the insertion of the instance as the first argument to methods is explained as something that Python just does for you: "Don't worry about it, it just happens!".
> More intermediate Python programmers typically get so used to self that they hardly notice it anymore in their function signatures, let alone wonder about what's powering it.
>
> To explain this bit of Python magic, I’ll give you an informal introduction to something called descriptors.
> To be sure, this talk isn’t going to be an in-depth discussion of the finer details of the descriptor protocol.
> Rather, it’s aimed at advanced beginners and intermediate Python developers who are eager to get an idea of what lies beneath the surface of Python.
> With this talk, I hope to pique your curiosity about the more advanced features of the Python programming language and hopefully give you a glimpse of all the things that are possible.

- creating a method in a class actually creates a normal function and the class has an attribute (of the same name) that points to the function
- an instance of the class gets a *bound method* as it's attribute
    - the instance has already been inserted as the first parameter

```python
class Guitar:
    def __init__(self, name: str) -> None:
        self.name = name

    def play_note(self, note: str) -> None:
        print(f"My {self.name} plays the note {note!r}.")
```

```python
Guitar.play_note
```

    <function __main__.Guitar.play_note>

```python
warwick = Guitar("Warwick Streamer")
warwick.play_note
```

    <bound method Guitar.play_note of <__main__.Guitar object at 0x7f403f26c190>>

- descriptors:
    - objects that modify what happens when we do something with an attribute
        - customize lookup, assignment, and deletion of attributes
    - implement the "Descriptor Protocol"
        - `__get__` method customizes lookup: `warwick.play_note`
            - automatically provided
        - `__set__` method customies assignemnt: `warwick.play_note = "value"`
        - `__delete__` method customizes deletion of an attr: `del warwick.play_note`
    - below is an example of implementing a descriptor in the `Guitar` class that just tells if the instance is my favorite guitar, the Warwick Streamer
        - only implements the dunder `__get__` method with three parameters:
            - `self`: the `FavoriteDescriptor` instance
            - `instance`: the instance of some class that has the attribute (`warwick` in this example)
                - if `None`, then the `FavoriteDescriptor` is returned (common practice in Python)
            - `owner`: the class that owns the attribute

```python
class FavoriteDescriptor:
    def __get__(self, instance, owner):
        if instance is None:
            return self
        return instance.name == "Warwick Streamer"


class Guitar:
    def __init__(self, name: str) -> None:
        self.name = name

    def play_note(self, note: str) -> None:
        print(f"My {self.name} plays the note {note!r}.")

    is_my_favorite = FavoriteDescriptor()
```

```python
Guitar.is_my_favorite
```

    <__main__.FavoriteDescriptor at 0x7f403f26d050>

```python
warwick = Guitar("Warwick Streamer")
warwick.is_my_favorite
```

    True

- now looking at the `__get__` method for `function` in Python

```python
class function:
    def __get__(self, instance, owner):
        if instance is None:
            return self
        return PyMethod(self, instance)
```

```python
Guitar.play_note
```

    <function __main__.Guitar.play_note>

```python
Guitar.play_note.__get__(None, Guitar)
```

    <function __main__.Guitar.play_note>

```python
warwick.play_note
```

    <bound method Guitar.play_note of <__main__.Guitar object at 0x7f403f1b11d0>>

```python
warwick.play_note.__get__(None, Guitar)
```

    <bound method Guitar.play_note of <__main__.Guitar object at 0x7f403f1b11d0>>

```python
warwick.play_note.__get__(warwick, Guitar)  # what normally happens behind the scenes
```

    <bound method Guitar.play_note of <__main__.Guitar object at 0x7f403f1b11d0>>

- other built-in descriptors (often used as decorators):
    - `classmethod`: to bind the class, not the instance, to a function
    - `staticmethod`: to return the function "as-is", not bound to a class nor function
    - `property`: easily create getters/setters/deleters

## Using Declarative Configs for Maintainable Reproducible Code

⭐⭐⭐⭐

**Jonathan Striebel** ([recording](https://youtu.be/omhJrT90lXU))

> Wondering how to keep your application config from getting outdated?
> Looking for a way to future-proof it in a backwards-compatible manner, keeping previous versions reproducible?
> Join this talk, we’ll share how declarative configs can be leveraged to make your code maintainable and reproducible at the same time.
>
> Therefore, an overview across the application config landscape is given – from inputs as cli-args, env-vars, and config-files, to their representations in code, covering serialization & deserialization, type-safety with config-schemas and evolutions.
> We’ll recommend cherries to pick for a maintainable and expressive declarative config system.

- maintainability and reproducibility
    1. separatation of config and code
    2. config verification - make sure the config only uses available options
    3. code verification - make sure the code uses the connfig correctly
    4. automated migrations
- keep config declarative
    - data only
    - no logic branches (e.g. if-statements)
    - let the Python code by imperative and driven by the config
- input formats:
    - command line arguments (gets tricky with many configurations)
    - environment variables
    - YAML, JSON, TOML, INI
        - read into Python as a class
        - use the `attr.s(auto_attribs=True)` to automatically generate the `__init__()` and some other methods
        - use the 'cattrs' library for converting the deserialzed YAML file into the `ConfigSchema` object
        - 'pydantic' is also a good option instead of 'attrs' and 'cattrs'
        - use a type checker to validate

```python
import attr
from pathlib import Path

@attr.s(auto_attribs=True)
class ConfigSchema:
    path: Path
    threshold: int
    plot_x: str
    plot_y: str
```

- include version numbers for creating automatic migrations as the configuration evolves
    - each time the configuration changes, create a new function that updates the previour version to the new one

## Large Scale Data Validation (with Spark and Dask)

⭐⭐⭐

**Kevin Kho** ([recording](https://youtu.be/2AdvBgjO_3Q))

> Data validation is checking if data follows certain requirements needed for data pipelines to run reliably.
> It is used by data scientists and data engineers to preserve the integrity of existing workflows, especially as they get modified.
> As an example, extreme machine learning predictions can be stopped from being displayed to application users if a new model is bad.
> Missing data can be flagged if it has the potential to break downstream operations.
>
> As data volume continues to increase, we will examine how data validation differs between a single-machine setting and a distributed computing setting.
> We will show what validations become more computationally expensive in Spark and Dask.
> For large scale data, there is sometimes also a need to apply different validations on different partitions of data.
> This is currently not feasible with any single library.
> In this talk, we will show how we can achieve this by combining the strengths of different frameworks.
>
> To demonstrate the data validation journey, we'll go over a fictitious case study.
> The data will start small, and we'll apply Pandas-based validations with ['Pandera'](https://pandera.readthedocs.io/en/stable/) and ['Great Expectations'](https://docs.greatexpectations.io/en/latest/intro.html) while discussing the pros and cons of each.
> As data size increases, we'll go over in detail the pain-points of transitioning to a distributed setting.
> We'll show one way to reuse the same Pandas-based validations on Spark and Dask by wrapping them with ['Fugue'](https://fugue.readthedocs.io/en/latest/introduction.html).

- notes on ['Great Expectations'](https://docs.greatexpectations.io/en/latest/intro.html):
    - works with 'Spark' and 'pandas'
    - can automatically create a "data document" showing statistics and validations for a checked data set
    - detailed outputs
    - fancy notifications available (e.g. through Slack)
    - CLI
- notes on ['Pandera'](https://pandera.readthedocs.io/en/stable/):
    - more light-weight than 'Great Expectations'
    - only works on 'pandas'
    - simple to add custom validators
    - decorators to wrap functions functions with validation schemas
- notes on ['Fugue'](https://fugue.readthedocs.io/en/latest/introduction.html):
    - > It is a pure abstraction layer that adapts to different computing frameworks such as Spark and Dask. It is to unify the core concepts of distributed computing and to help you decouple your logic from specific computing frameworks.
    - can use with 'pandera' to validate data in a scalable fashion

## Unexpected Execution: Wild Ways Code Execution can Occur in Python

⭐⭐⭐⭐

**Graham Bleaney, Ibrahim Mohamed** ([recording](https://youtu.be/R1KrPc1IOeE))

> Every Python user knows that you can execute code using eval or exec, but what about `yaml` or `str.format`?
> This talk will take you on a walk through all the weird and wild ways that you can achieve code execution on a Python server (and trust me, I didn’t spoil the surprise by putting the weirdest ones in the description).
>
> The talk should be equal parts practical and entertaining as we work through both real examples of code execution vulnerabilities found in running code as well as absurd remote code execution exploits.
> The talk will end on a practical note by explaining how Facebook detects and prevents the exploit vectors we discussed, using an open source 'Python Static Analyzer' called ['Pysa'](https://engineering.fb.com/2020/08/07/security/pysa/).

(Interesting, but I didn't take notes because it is not directly relevant to my current work.)

## Patterns of ML Models in Production

⭐⭐⭐⭐

**Simon Mo** ([recording](https://youtu.be/GJt89jCiPUQ))

> You trained a ML model, now what?
> The model needs to be deployed for online serving and offline processing.
> This talk walks through the journey of deploying your ML models in production.
> I will cover common deployment patterns backed by concrete use cases which are drawn from 100+ user interviews for [Ray](https://docs.ray.io/en/master/index.html) and [Ray Serve](https://docs.ray.io/en/master/serve/index.html).
> Lastly, I will cover how we built Ray Serve, a scalable model serving framework, from these learnings.

- architectual patterns:
    - *batch*: model is run on a schedule by workflow management system on new batches of data
    - *stream*: incomming events are constantly coming in and run through the model in small batches
    - *serve*: models are served in real time, interactively
        - superset of 'batch' and 'stream'
- most common patterns of ML in production:
    - *pipeline*
        - multiple models process an input sequentially
        - each step handles a different part of the problem solving process
        - pros:
            - separation of concerns
            - each model can be updated separately
        - cons:
            - depedency of models makes deciding how to retrain models tricky
            - difficult to parallelize and scale
    - *ensemble*
        - multiple versions of the model deployed
        - aggregation of predictions from multiple models
        - dynamic selection of certain models based on features of the data
    - *business logic*
        - every part of the app not directly invovled in inference
            - e.g.: database lookup, data/feature prep and validation
        - often requires a lot of network activity, making the server network-bound and I/O heavy
            - the model inference is already compute-bound and memory hungry, making the overall server very expensive
        - common to split into separate services, but now difficult to manage communication and keeping them in sync
    - *online learning*
        - the model in deployment is still being trained and validated
        - tricky to do properly

## The Road to Pattern Matching in Python

⭐⭐⭐

**Tobias Kohn** ([recording](https://youtu.be/NYUXNRswe6k))

> Pattern matching is a great and proven tool for programmers.
> However, can we also assimilate and integrate it into Python?
> This talk tries to give an answer and discusses the rationale and ideas behind the recent "pattern matching" PEPs.
>
> Processing structured data has sparked ever more powerful programming tools.
> Python's objects and classes, for instance, have proven themselves to be particularly versatile and form part of the backbone of the language.
> Constructing or building new objects — including built-ins such as lists, tuples or dictionaries — abounds in any Python code.
> In contrast, testing the structure of data and extracting specific elements is often rather cumbersome, requiring the frequent use of built-in functions like `isinstance`, `len` and `getattr`.
>
> Pattern matching addresses this issue by introducing a new paradigm to de-construct data, complementing existing tools.
> It can be thought of as an extension of Python's iterable unpacking to arbitrary objects.
> However, it does so in a 'safe' way, ensuring that objects have the necessary structure to proceed with unpacking elements and attributes.
>
> The objective of this talk is to give you an overview of why pattern matching matters and what it really is.
> You will gain a deeper understanding of the core concepts that make up pattern matching, as well as the design decisions and ideas behind the recent "pattern matching" PEPs.
> However, this talk will not provide an introduction on how to use pattern matching in your code, nor is it about the intricacies of the implementation.
> If you are a Python programmer, have heard of the new pattern matching features and are wondering what it is all about, then this talk is for you.

- pieces of pattern matching:
    - checks the structure/shape/type of the data
    - selects code to handle a specific object
    - extracts relevant pieces of information
- meaning of pattern matching cases:
    - simple names are binding targets
    - attributes provide value constrains for the patterns
    - binding targets must be determinisitic

### Conclusion

This was a very interesting talk about pattern matching in more general terms than just its implementation in Python.

## What are quantum computers, and how can we train them in Python?

⭐⭐

**Josh Izaac** ([recording](https://youtu.be/o377m0doD6M))

> Quantum computers aren’t what-ifs anymore — they are available now, and publicly accessible over the internet.
> And Python is rapidly becoming the language of choice for accessing and programming quantum computers, with Python SDKs available from Google (Cirq), IBM (Qiskit), and others.
> However, early quantum computers are small, noisy, and error prone. Simultaneously, it has never been easier to perform differentiable programming in Python; simply swap out NumPy for TensorFlow, PyTorch, or JAX, and you have the ability to differentiate and train the program itself.
>
> So what would happen if we attempted to combine the two?
>
> Using a mixture of real Python examples and illustrated diagrams, we show how to not only evaluate, but also differentiate small quantum programs directly on quantum hardware.
> By extracting the gradients, we can integrate these quantum programs directly into larger differentiable programs in Python, and train/optimize the full (hybrid quantum-classical!) program.
> Over the course of this talk, quantum-curious Python developers will see first-hand how quantum programming looks in Python, and get an idea of how (and when) it makes sense to take advantage of these novel hardware devices.

- quantum computers:
    - many ways to build a quantum computer
        - currently use "near-term" quantum computers which are small and noisy
    - take advatnage of quantum properties such as *entanglement* and *superposition*
    - suggests thinking of near-term quantum devices as "black boxes" that perform matrix-multiplication very well
        - extract classical data from this black box via measurement statistics

## Getting an Edge with Network Analysis with Python

⭐⭐⭐⭐

**Alon Nir** ([recording](https://youtu.be/DPqhV21tSAc))

> Networks are all around us. While Facebook and Twitter are the obvious examples, every time we shake hands, drive from point A to B, push code to github, check out a meetup or rate a show on IMDB, we’re participating in network activity.
> People, places, things and even ideas are inter-connected in innumerable networks, and these can have a great (yet sometimes inconspicuous) impact on our lives.
>
> The purpose of this talk is to introduce members of the audience to network analysis and its importance, and give them the basic building blocks for applied network analysis with Python.

This was a simple introduction to network analysis with ['networkx'](https://networkx.org) library.

```python

```
