# C Unit Test Style Guide

* Be consistent with indentation! Typically you indent in once every time a curly bracket is opened and out when the bracket is closed. Programs like Atom will do this automatically for you.
* Notation, notation, notation! Clearly explain what you are testing __and__ the expected outcome.
* On the line preceding each main test (TEST{}), please write a not describing what function this test is testing.
* Within the TEST({}) brackets the format for naming the  test is (NameofDoc, NameOfFunction). For example, (SWFlowTest, potSoilEvapBS).
* Inputs should be declared first in the TEST. You should try and minimize the number of lines input declaration consumes. For example, you can declare all double that are a length of 1 on one line.
* There should be only one definition of a variable before each unit test. Avoid declaring it in the inputs and then defining it again before a unit test, without the variable ever being called.
* Break your TEST code into blocks based on different test conditions (i.e. input X > input Y). Write text (i.e. “Testing when biolive is greater than shade”) and then put everything (defining or re-declaring inputs, running the function, assertions) below this text. For example:

```
     // Inputs
     double biolive, shade;

     // Test when biolive is greater than shade
     biolive = 400;
     shade = 300;

     result = bioliveFunc(biolive, shade);
     EXPECT_GT(0, result); // We expect the results to be 0 when biolive is greater than shade.
 ```
* Use a sensible and consistent amount of line spaces: i.e. one space between inputs and functions, or when you are setting different conditions etc.
