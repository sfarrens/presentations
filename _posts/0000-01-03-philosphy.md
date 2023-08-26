## Software design philosophy

--

Here are some things to consider when starting a project.

--

# ⚠️ Think before you code!

<background>#E55B3D</background> 

--

Some important questions to ask yourself are:

+ Who is this code for?
+ What is the scope of this code?
+ How should this code be used?
+ Where will this code (likely) be run?
+ How long will this code (likely) be used?

--

> Spending some more time early on answering these questions could save us a lot of time in the long term.

#### 🧐

--

> Let's look at a very simple example. Can you guess what the following function is for? 🤔

```python
def func(a):
    b = e = 0.7
    c = 1.0 - b
    d = 0.0
    g = (1 + a)
    x = b**2
    y = c * g**3 + d * g**2 + e

    return np.sqrt(x * y)
```

--

> What if we rewrote the function as follows, is it more clear now? 💡

```python
def hubble(redshift):
    hubble_const = 0.7
    matter = 0.3 * (1 + redshift) ** 3
    curvature = 0.0 * (1 + redshift) ** 2
    dark_energy = 0.7

    return np.sqrt(hubble_const**2 * (matter + curvature + dark_energy))
```

--

> This is just a very simplistic example of the thought we can put into the development of our software that will make it easier to use, understand and maintain.

> Throughout this lesson we will build upon this simple example to arrive at something that more closely resembles high quality scientific software.

> So, let's get to work! 💪