# openErp7-documentation
When Read the freaking manpage and use the source Luke not working, your **w**rite **t**he **f**ancy **man**ual(WTFM, or WTFMan).

Not gonna update anymore.
Feel free to folk or c&p as you like.



# Lab 5

1. Create a function `rollAdice`. The function receives the number of faces of the dice and returns the value once the dice is rolled. Assume the number received by the function is a positive integer value.

2. Create a function `printOneLine`. The function receives two parameters: the number of spaces to print twice, and the character to print between the spaces. Assume the number of spaces received is a positive integer value.

   As an example 

   `printOneLine(3,'x')` prints `---x---` (where - represents the space character)

3. Create a function `isOdd` that receives an integer value and that returns true is the value is odd, false otherwise. Assume the number received is an integer value.

4. Create a function `xMasTree` that receives an odd integer value and a character that displays a Christmas tree using the character as the display character. The function needs to use the function `printOneLine`. As an example, `xMasTree(7,'x')` produces the following output:

      x   
     xxx  
    xxxxx 
   xxxxxxx

   

5. Create a test program (driver) that proposes a menu to the user with 5 options:

   - Option 1 is to test the `rollAdice` function
   - Option 2 is to test the `printOneLine` function
   - Option 3 is to test the `isOdd` function
   - Option 4  is to test the `xMasTree` function
   - Option 5 is to Exit

   Inside each option except 5 you will prompt the user for values, perform error checking on the values provided (using `try except`), call the function, display the result then ask the user if he/she wants to start again.

6. Once finished, upload your lab to brightspace.



