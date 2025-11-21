# The program
```java
public class FunQuiz {

    public static void main(String[] args) {

        int a = 100;
        int i = 0;

        class AwersomeClass {
            int run(int num){
                return a + num;
            }
        }

        do {
            if (i == 0)
                if (Double.NaN != Double.NaN)
                    i += 012;
                else
                    a++;
        } while (false);

        var awesomeClass = new AwersomeClass();

        System.out.println(awesomeClass.run(i));
    }
}
```
Now lets see why it evaluates to `110`
# Lines 3-4
```java
int a = 100;
int i = 0;
```
I mean, it initializes and assigns the variables, nothing special

# Lines 5-9
```java
public class FunQuiz {
    public static void main(String[] args){
        ...
            class AwersomeClass {
                int run(int num){
                    return a + num;
                }
            }
        ....
    }
}
```
The class is defined inside the method of a class, thus it is an <b>local inner class</b>.
This measn that all variables that are used in its definition must be either final or effectively final, where the latter means that the value is never mutated.

# Lines 10 & 16
```java
do {
    ...
} while (false);
```
A `do` loop checks the condition at the whether to loop again, and since that it false. It will just run the inner block once and then terminate. So it is redundand and can be removed.

# Lines 11 - 15
```java
if (i == 0)
    if (Double.NaN != Double.NaN)
        i += 012;
else
    a++;
```
Java matches the last if statement with the first else statement. Thus when we put braces around it it will look as the following:
```java
if (i == 0){
    if (Double.NaN != Double.NaN){
        i += 012;
    }
    else {
        a++;
    }
}
```
Now `i == 0` is abviously true since we defined it that way<br>
The statement `Double.NaN == Double.NaN` is false since that's the way it's defined in [IEEE 754](https://www.geeksforgeeks.org/computer-organization-architecture/ieee-standard-754-floating-point-numbers/). 
Thus the statement `Double.NaN != Double.NaN` is true since it is the same as `!(Double.NaN == Double.NaN)`.<br>
<br>
<b>Now if the if statement was not true</b>, we would execute `a++;` and since we used that in the local inner class; this would mean we used a variable that is not effectively final in the local inner class and thus a compile error would be thrown. <br>
But since this statement is now unreachable the compiler still considers `a` as effectively final. Now because the if statement is true, `i` is increment by 012. Which is 12 in octal converted to decimal which is $1*8^1+2*8^0=10$.

# Lines 17-18
```java
var awesomeClass = new AwersomeClass();

System.out.println(awesomeClass.run(i));
```
We construct an AwesomeClass and call run with parameter `i`, which does `a+i`. Since `a=100` and `i=10`. The final number that will be printed is `110`;