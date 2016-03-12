## Reverse Polish Notation Calculator

Your assignment today is to implement a program that functions as a calculator. This won't be just any calculator, though, but a Reverse Polish notation calculator. There's a good chance you've never heard of this particular flavor of calculator, so here's a quick overview.

### Reverse Polish Notation

Many of us are solely used to entering computations into a calculator by putting an operation between two values. For example:  

| value | operation | value |
|:-----:|:---------:|:-----:|
|   3   |     +     |   4   |

However, there are other notations that can be used to express mathematical operations. Two of these are *prefix* and *postfix*. *Prefix* notation is also known as *Polish Notation*, and *postfix* notation is also known as *Reverse Polish* notation. Today we will be building a calculator that implements Reverse Polish Notation ([RPN](https://en.wikipedia.org/wiki/Reverse_Polish_notation)). For example, instead of writing `3 + 4` as we did above, we would write `3 4 +`, like this:  

| value | value | operation |
|:-----:|:-----:|:---------:|
|   3   |   4   |     +     |

This method of inputting computations actually makes it easier to write logic to perform the operations (for the computer, anyway). This is because the format lends itself to storing computations in a [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)). A stack is a specific type of *queue* (know as last in, first out, or *LIFO*)  which only supports two types of operations, pushing and popping.

You can think of a stack as a list with less functionality. (**Note**: the [deque](https://docs.python.org/2/library/collections.html#collections.deque) class is the closest native Python class to a stack. When you go to implement you're version of a RPN calculator today, you can choose to use this class instead of a `list`, as it will technically give you speed gains on the pushing and popping operations you will need to perform over the same functionality being implemented with a list.) What a RPN calculator does is accept input one element at a time and pushes them in turn onto the stack. These elements can either be values or operations. In Python list lingo, pushing to the stack just means appending them to a list. When the user wants to perform an operation, the values necessary to perform the operation are [popped](https://docs.python.org/2/tutorial/datastructures.html) off the stack, in turn. The value resulting from that operation is then pushed back onto the stack. If it is the last value on the stack, then that value is returned. Otherwise, the process starts over again.

### RPN Example

Say we want to calculate the value resulting from the calculation: `6 + ((5 + 3) / 4) - 3`. In RPN, that calculation is written: `6 5 3 + 4 / + 3 −`. To input this calculation into a RPN calculator, one would enter those elements, in turn, and the corresponding operations would occur in the following order.

| Input |  Operation   |    Stack    |                   Other                   |
|:-----:|:------------:| ----------- |:-----------------------------------------:|
|   6   |  Push Value  | [ 6 ]       |                                           |
|   5   |  Push Value  | [ 6, 5 ]    |                                           |
|   3   |  Push Value  | [ 6, 5, 3 ] |                                           |
|   +   |     Add      | [ 6, 8 ]    | Pop two values (3 then 5), push result, 8 |
|   4   |  Push Value  | [ 6, 8, 4 ] |                                           |
|   /   |    Divide    | [ 6, 2 ]    | Pop two values (8 then 4), push result, 2 |
|   +   |     Add      | [ 8 ]       | Pop two values (2 then 6), push result, 8 |
|   3   |  Push Value  | [ 8, 3 ]    |                                           |
|   -   |   Subtract   | [ 5 ]       | Pop two values (3 then 8), push result  5 |
|       |    Return    |   5         |                                           |

If you want to play around with a RPN calculator to get a feel for them and how they operate, check out [this](http://www.meta-calculator.com/learning-lab/reverse-polish-notation-calculator.php) link.

### RPN Calculator

Your assignment is to implement a calculator that operates with Reverse Polish notation. This can be done with solely functions, and while this is supposed to be an OOP lab, you're welcome to start off by writing functions that solve the problem; in fact it makes a lot of sense. At some point in the assignment, though, there will be a part that nearly necessitates implementation of the calculator with OOP.

The following steps are suggestions, and while they could possibly make your journey through this programming assignment easier, they do not need to be followed. Remember, there are always multiple ways to solve programming problems. Sometimes there's an obvious solution, sometimes not so much. Sometimes there's a more elegant solution, sometimes they all function about the same. At a high level, the purpose of this lab is to have you work through a problem, on your own, start to finish, so you can get a good feel for what that process is like. In addition, the instructors will be around if you have any questions or concerns about how you are wanting to implement your solution.

You are encouraged to talk about the problem with your classmates and/or the instructors at any time; frequently one of the biggest hurdles when trying to solve a problem is trying to figure out what the problem actually is. Trying to explain it to someone, and having a conversation about it, can go a long way towards understanding the problem fully.

#### Step 1: Interact with the Problem at a High Level

Take a look at wikipedia's description of the [algorithm](https://en.wikipedia.org/wiki/Reverse_Polish_notation#Postfix_algorithm) for evaluating a string of operations in RPN. This is a great outline of what your program will need to be able to do. Follow along with the example, making sure you understand how the algorithm is evaluating the sequence and what the state of the stack is at each step.

#### Step 2: Devise a Plan

With the RPN algorithm for computing a string of numbers and operations in mind, you're going to want to write a function that takes a string of characters and computes the result of the computation that the string of characters gives.

In line with the top-down tactics that we have previously discussed, consider how this problem can be broken down into smaller pieces that are easier to be solved. What will the big problem be then? Consider making a function that will take a string of elements, separated by spaces, and evaluate them with a stack like storage procedure (LIFO queue). Some questions to ask yourself:

* What would that function look like? 
* What would you call it? 
* What parts of what it needs to do could be assigned to other functions? 
* What would they be called?
* What structure(s) are you going to use to store the data?

Take a look at [this](http://codereview.stackexchange.com/questions/79795/reverse-polish-notation-calculator-in-python) stack exchange post. Notice how this solution uses a list to keep track of the state of the stack. As it evaluates the elements in the calculation it pops, evaluates, and inserts as dictated by the algorithm until the calculation is completed. As one of the comments on the post says, lists are optimized to grow and shrink from their end. While they support popping and inserting at any index, these operations are computationally inefficient.

This code is pretty decent, except for way it keeps track of the stack. Consider understanding what that solution is doing and using it as a starting place for the next step. Make sure that you don't use the same methods it uses to keep track of the stack. (i.e. popping/inserting from/at somewhere other than the end of the list). You can still use a list to keep track of the state of the stack, but make sure you only alter from the end of the list. You could also use the `deque` class that was mentioned above. Deque's are optimized to grow from both their beginning or end; this behavior could be useful.

You can also see the use of the `operator` library. This class turns the standard mathematical calculations you write, `+` and `-`, into functions, `sum()` and `sub()`, respectively. These functions can make your life easier as you're trying to implement your program. Note, too, that the linked solution provides more complicated operations (`sin`, `tan`, `cos`, and `pi`). You don't need to implement these if you don't want to, for simplicity's sake. Remember, you can always add functionality later.

Once you have looked at the code and answered some or all of the above questions, you are well on your way to writing some pseudocode.

#### Step 3: Pseudocode

Now that you've moved from the outline of the algorithm to thinking about how you will task out parts of it to functions, you are in a great position to begin pseudocoding. Pseudocode can take on a variety of different appearances. It can be simple notes to yourself about steps you want to take. It can be instructions that are semi-code like (e.g. if you're going to loop over some container you might write a line of pseudocode that looks like `for thing in stuff`, giving better names to each `thing` and the `stuff`). It can be almost functioning code.

The nice thing about pseudocode in Python is that you can often write in nearly accurate Python syntax. While variables you want to reference or functions you want to call might not exist yet, this isn't a problem because its just pseudocode! Then when you want to go fill in the pseudocode, and make it "real" code, you already have some lines that will work in Python. In addition, you should have great names for everything you'll need to name. For example, if I'm trying to solve a problem that needs to load a list of words, and then loop through that list and make a dictionary with the number of words of different counts, my pseudocode might look like the following:

```python
def get_word_count_dict():
    word_list = load_word_list()
    count_dict = make_count_dict()
    return count_dict


def load_word_list():
    with open get file(s):
        load words from files to list
    return word_list


def make_count_dict():
    for word in word_list:
        build up dictionary
    return dictionary
```

Now that this skeleton exists, it's easier to see what types of parameters each function will take, and think about how the whole program will flow. You might notice that both `load_word_list()` and `make_count_dict()` will need to accept parameters. Now that we've written this pseudocode, though, that's way easier to see than when you first thought about how to solve the problem.

#### Step 4: Implement Real Code

At this point you should have a really good, nearly code-like structure for your program to create a function to calculate a RPN operation. Now you can go through and make the pseudocode working code by filling it in. Along the way, you'll start seeing places where you can make functions out of certain routines that do their own thing and/or are run in a loop, and that's great! You're well on your way to a working program.

#### Step 5: Debug

Once you get your code to a state where it can run, you're at the point of debugging. There's a chance that you won't need to go through this step, but it is way more likely that there will be some small error or something you didn't account for in your first solution.

Don't fret - this is part of the programming process, and even the best programmers have to debug their code. Embrace it. It means you're making mistakes and learning!

There are many methods by which you can debug. You can read through your code for syntax/logic errors, or you can print things out to make it easier to follow the flow of your program as it actually runs. You can also use the Python Debugger, or [pdb](https://docs.python.org/2/library/pdb.html) for short. This tool can be really helpful if you want to interact with your program similarly to the way you interact with IPython. `pdb` allows you to pause the Python interpreter while it is running your script so that you have a chance to poke around and check the state of your program, the variables that exist, what their values are, etc, at any step. You can use `pdb` at any point in your programs execution (you just have to use it the right way). Check out this [blog post](https://pythonconquerstheuniverse.wordpress.com/2009/09/10/debugging-in-python/) for a great intro on using this tool.

#### Step 6: Building the User Interface

Now you have function that will parse a string of operations. How will a user of your calculator interact with this function? Are they going to be prompted for input, or do they have to give it to the function in the script? The former is suggested, and remember that you can use [raw_input](https://docs.python.org/2/library/functions.html#raw_input) to get this information from the user.

In addition, will the user be able to enter more than one calculation? If so, how can you use a loop to have your function called over and over? There's a tactic we use in Python when we can't foresee how long a loop is going to last - we'll use a `while True` statement and have logic in the loop determine when to `break` out of it. Since the `while True` will never trigger `False` so it will never end until manually broken out of.

Putting your function in a loop might change the way it needs to interact with the user. This change might involve printing a result in a different part of the program. It may involve changing what is returned from one of your functions to then be displayed. Again, there is no right answer - you'll just need to think critically about how your program is running. And test! Testing is what good programmers do, and they do it often.

#### Step 7: A Better Calculator

Now you have a way to calculate an arbitrary RPN style string of operations. But a real calculator does more - it keeps track of the state of your stack and allows the user to decide when they want to evaluate the stack. It allows them to arbitrarily add values and operations even if there are still values left on the stack. 

In order to implement functionality of this nature, you will need to persist the stack in some way. This can be done with functions or with OOP. Consider the tenants of OOP and how they might fit in with this new version of the problem. Which method of solving the problem do you think will fit better? It seems like OOP fits well, and the rest of the assignment is built on that assumption; however, if you think that functions will be sufficient or easier than OOP, then by all means tackle the problem that way!

With OOP in mind, we now need to move our current functionality into a class. We obviously have some methods, though, that will need to be changed; what attributes will our class need to have?

#### Step 8: Implement a Class

Time to transition from procedural code to OOP code. You've thought about the attributes of the class and have some existing functions; now you need to make the switch. As you do this think about how you can take advantage of the pluses of OOP to make your solution more elegant.

You'll probably quickly see that having the stack stored as an attribute will make your life easier. But how are we going to make a real calculator?

#### Step 9: Persist the Stack

You will almost certainly have to change some implementation details of how your function evaluates RPN strings since we now have to consider what's on the stack already. This also means that we have to be ok with things remaining on the stack after we try to evaluate it. Making this work will be the crux of getting full calculator-like functionality in your program. Once you get to this section it might be worth talking to an instructor to discuss different tactics for approaching this problem.

Remember, there are many ways to solve this problem, and while there will be a solution posted for you to look at, it is definitely worth you time to struggle through this problem and learn from it while you have the instructors to talk to you about tactics.

To get you started off on a potential foot, consider having the starting point of users interacting with your class be a `run()` method. This will enter them into a `while True` loop that continually allows them to interact with the calculator in different ways.

The solution allows for operations to be pushed onto the stack indefinitely. When the user wants to evaluate the stack they merely have to enter nothing when prompted. Once the stack has been executed as fully as possible it the calculator will display the state of the stack. If at any time the user wants to see what's on the stack they can enter `s`. If they want to clear the stack entirely they can enter `c`. And if they want to quit the calculator they can enter `q`. Take a look at what interacting with working solution could look like:

```
Operation(s)/Value(s): 4 5 +
Operation(s)/Value(s): s
Stack: 4.0, 5.0, +
Operation(s)/Value(s):  
Stack: 9.0
Operation(s)/Value(s): 7 sqrt
Operation(s)/Value(s): s
Stack: 9.0, 7.0, sqrt
Operation(s)/Value(s): 
Operation(s)/Value(s): s
Stack: 9.0, 2.64575131106
Operation(s)/Value(s): c
Operation(s)/Value(s): s
Stack: empty
Operation(s)/Value(s): q
```

### Extra Credit

Try to break your solution by entering calculations that make no sense. Can you make your program robust to these poorly/incorrectly formatted inputs? It's good to think about these things, though frequently it's very difficult to foresee all the possibilities of bad input. So don't drive yourself crazy trying to protect against everything.