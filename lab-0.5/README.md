# Lab 0.5 - Solid Practises, Code Quality and Angular tools

What’s the meaning of code quality and what the *react-hell* are good practices?

Why can’t I start directly copying and pasting code from Stack Overflow?

<br/>
<p align="center">
<img src="./resources/mug.png" width="300">
<br/>
**Code is my life, forget anything else!**
</p>
<br/>

If you think this way, imagine the next scenario:

> We want to sell a web page to a client for a price. After some information exchange we tell the client the range of possible prices before entering into the details for a better estimation.

> Some time later, the client tells us that the son of a neighbor can do the same for half the price. We offer our advice against it not because of losing a business opportunity but because the neighbor’s son typical endings… but in the end, nothing can be done and that is.

<br/>

Imagine now the most typical result…
<br/>
<details>
 <summary>You'll Never Believe the Result, Click Here to Know More! (not a click-bait)</summary>
<p align="center">

> Yes, the result is the client getting something unusable.
> **And I'm not joking. I was a neighbor’s son.**
</p>
  </details>
<br/>

Client gets something that is not fulfilling the expectations at all, in most of the cases not even finished… forget about that son pain trying to create something complex with the simplified and limited point of view of a coder *(I’m not talking about a developer that are things completely different)…* and don’t talk about **reusability, security, extensibility, robustness, reliability, resilientness…**
<br/>

> Sure! Nice and funny names that **smoke-sellers** use freely to try to sell the “same” I can do but far away more expensive - *That’s what I could say 20 years ago.*

## But why now, as an architect I insist on THAT is the key for success?
Creating a solution is not a matter of only coding . It requires a lot of **subjects** to consider, **objectives and targets** to be achieved, to think in a lot of candidate technologies that must work together aiming for those targets, deciding with one to use.

Change now the concepts names:
- subjects -> *functionalities*
- objectives and targets -> *requirements*

Only having those concepts clear (functionalities and requirements), technical decisions can be taken with solid foundation. 

So, again: **Don’t start coding as your first tasks.**

<br/>

# How to achieve success?

I know that removing Stack Overflow access will reduce your code skill stats by -20 points, my apologies, but before the urge of hitting keys is too strong…

<br/>
<p align="center">
<img src="./resources/huges.png" width="500">
<br/>
<br/>

- Designing a **Solid Architecture**.
- Applying software design **Main Principles**.
- Defining and measuring **Metrics**.
- Taking care of your **Team**. *Yes, this is not a technical thing but it's almost always forgotten and a key for success.*


<p align="center">
<img src="./resources/sense.png" width="200">
</p>
<br/>

Can you even see any mention to code? It's deep inside **Main Principles.**


## Solid Practices? 

I know that removing Stack Overflow access will reduce your code skill stats by -20 points, my apologies, but before the urge of hitting keys is too strong…

<br/>
<p align="center">
    <img src="./resources/boring2.png" width="440">
</p>




## SOLID DRY KISS YAGNI
We are not getting crazy, those are four of the main acronyms in software design principles.

<p align="center">
<img src="./resources/solid.png" width="700">
</p>

## Code Quality
Main key points for create and maintain good quality code.

**What Code Quality means is that your code:**
- Is documented for yourself and other developers.
- Is tested manually and automatically.
- Is extendible and the logic is break down in different layers and classes.
- Is secure, data is encrypted, user input data is validated, libraries and frameworks are up to date.
- Is unified, following coding guidelines and oficial styles.
- Is clear, easier to debug and maintain.

<br/>

**And what the project gets in exchange if you have high quality code?**
- Unifies code making it easier for new colleagues to incorporate to the project.
- Easier to debug not only for another developer but for the author itself.
- Produces robust and secure code.
- Reduce software maintenance cost.
- Generates quality product.



## Code Quality Rules
**All development tasks must follow the next rules:**
- Apply the TypeScript coding guidelines: Microsoft Coding guidelines 
- Use long and accuracy descriptive names, for classes, methods, constants, variables…
- Comments will follow JSDoc conventions. Comment all methods and classes, also all specific piece of code that requires explanation for future revisits or because of complexity.
- Create separate folders for each Ionic service group by logic.
- Isolate and publish only the minimum required for each class.
- Avoid to include any function or method that is not related to the class logic. Move them into a re-usable help class.
- Create Unit Test for every main functional requirement. The test must cover all cases.
- Use Async Functions. Do not chain Promises.
- Use let and const. Do not use var.
- Use double quotes always.
- Use === operator unless to compare with null or undefined.
- Use const with enum for string-based values. Do not use plain strings to compare.
- Once a big task has been finished, revisit it and apply refactorization whenever is possible.






Well, because the objective of this Openathon is Angular, I’ll leave it in this point for now,


# Main Tools
Introduction to the principal tools used real-world projects like: webpack-bundle-analyzer, CompoDoc, Codelyzer, Augury, dependency-check /npm-audit, SonarQube and sonar-scanner.
