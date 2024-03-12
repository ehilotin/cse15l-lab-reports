# Part 1 - Debugging Scenario

## The Student's Original Post
![Image](lr5step1.png)

**student123:**
> Hi. I’m not sure why this test is failing. It seems like maybe a loop is running too many times? But when I look at my code, I’m not sure what’s wrong with it.

## A TA's (Possible) Response
**ta001:**
> Hi there! It's hard to know exactly what's going on in your program with just this symptom. Your test details seem to show that your program's current runtime is not what you expected, so your guess at the bug may be correct. Can you think of any tools we’ve used in class that can help you figure out where your program has unexpected behavior?

## The Student's Second Take

## All Things Setup
**File & Directory Structure**
```
labreport5/
|- lab7/
  |- lib/
    |- hamcrest-core-1.3.jar
    |- junit-4.13.2.jar
  |- ListExamples.java
  |- ListExamplesTests.java
  |- test.sh
  |- ListExamples.class
  |- ListExamplesTests.class
  |- StringChecker.class
```
**File Contents Before Edits**
> Note: Used same repository as Lab 7 with slight modifications so JDB would run properly

*ListExamples.java*
```
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      index1 += 1;
    }
    return result;
  }


}
```
*ListExamplesTests.java*
```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;
import java.util.ArrayList;


public class ListExamplesTests {
	@Test(timeout = 500)
	public void testMerge1() {
    		List<String> l1 = new ArrayList<String>(Arrays.asList("x", "y"));
		List<String> l2 = new ArrayList<String>(Arrays.asList("a", "b"));
		assertArrayEquals(new String[]{ "a", "b", "x", "y"}, ListExamples.merge(l1, l2).toArray());
	}
	
	@Test(timeout = 500)
        public void testMerge2() {
		List<String> l1 = new ArrayList<String>(Arrays.asList("a", "b", "c"));
		List<String> l2 = new ArrayList<String>(Arrays.asList("c", "d", "e"));
		assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }

}
```
*test.sh*
```
javac -g -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```
**Commands Run**

To trigger the bug, I just ran the test.sh script using `bash test.sh`.

**Edits Made**

It's a relatively simple fix for something that might be a very confusing bug, since the code generally is properly written. On line 43 of `ListExamples.java`, `index1` should be changed to `index2`.

# Part 2 - Reflection
In the second half of the quarter, I was most interested by learning how to create bash scripts. I didn't know anything about scripting before this class, and now I think that scripting is a really efficient (and sometimes even fun) way to run programs. Learning about bash scripting really opened my eyes to a new world of possibilities in which I wouldn't have to type tons of commands just to achieve the same result as one script. Scripting will probably be my biggest takeaway from the second half of this class.
