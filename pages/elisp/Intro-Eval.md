

### 10.1 Introduction to Evaluation

The Lisp interpreter, or evaluator, is the part of Emacs that computes the value of an expression that is given to it. When a function written in Lisp is called, the evaluator computes the value of the function by evaluating the expressions in the function body. Thus, running any Lisp program really means running the Lisp interpreter.

A Lisp object that is intended for evaluation is called a *form* or *expression*[6](#FOOT6). The fact that forms are data objects and not merely text is one of the fundamental differences between Lisp-like languages and typical programming languages. Any object can be evaluated, but in practice only numbers, symbols, lists and strings are evaluated very often.

In subsequent sections, we will describe the details of what evaluation means for each kind of form.

It is very common to read a Lisp form and then evaluate the form, but reading and evaluation are separate activities, and either can be performed alone. Reading per se does not evaluate anything; it converts the printed representation of a Lisp object to the object itself. It is up to the caller of `read` to specify whether this object is a form to be evaluated, or serves some entirely different purpose. See [Input Functions](Input-Functions.html).

Evaluation is a recursive process, and evaluating a form often involves evaluating parts within that form. For instance, when you evaluate a *function call* form such as `(car x)`, Emacs first evaluates the argument (the subform `x`). After evaluating the argument, Emacs *executes* the function (`car`), and if the function is written in Lisp, execution works by evaluating the *body* of the function (in this example, however, `car` is not a Lisp function; it is a primitive function implemented in C). See [Functions](Functions.html), for more information about functions and function calls.

Evaluation takes place in a context called the *environment*, which consists of the current values and bindings of all Lisp variables (see [Variables](Variables.html)).[7](#FOOT7) Whenever a form refers to a variable without creating a new binding for it, the variable evaluates to the value given by the current environment. Evaluating a form may also temporarily alter the environment by binding variables (see [Local Variables](Local-Variables.html)).

Evaluating a form may also make changes that persist; these changes are called *side effects*. An example of a form that produces a side effect is `(setq foo 1)`.

Do not confuse evaluation with command key interpretation. The editor command loop translates keyboard input into a command (an interactively callable function) using the active keymaps, and then uses `call-interactively` to execute that command. Executing the command usually involves evaluation, if the command is written in Lisp; however, this step is not considered a part of command key interpretation. See [Command Loop](Command-Loop.html).

***

#### Footnotes

##### [(6)](#DOCF6)

It is sometimes also referred to as an *S-expression* or *sexp*, but we generally do not use this terminology in this manual.

##### [(7)](#DOCF7)

This definition of “environment” is specifically not intended to include all the data that can affect the result of a program.
