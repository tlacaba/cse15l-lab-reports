# Week 3 Lab Report: Simple Search Engine and Debugging

## Week 2: Simple Search Engine

During week 2's lab, I created a simple search engine that worked on my local computer.

This simple search engine worked by handling URLs and interpreting the path as calls to certain subroutines. I programmed it to be able to add strings to an ArrayList and to be able to take search queries, looking through the ArrayList for any strings containing the query.

Here is the code that was used to implement it:
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The ArrayList that will be searched through
    ArrayList<String> searches = new ArrayList<>();

    public String handleRequest(URI url) {
        System.out.println("Path: " + url.getPath());
        if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if(parameters[0].equals("s")) {
                searches.add(parameters[1]);
                return String.format("Added %s to searches!", parameters[1]);
            }   
        }
        else if (url.getPath().contains("/search")) {
            String[] parameters = url.getQuery().split("=");
            if(parameters[0].equals("s")) {
                String searchQuery = parameters[1];
                String outputString = "Results: ";

                for (int i = 0; i < searches.size(); ++i) {
                    if (searches.get(i).contains(searchQuery)) {
                        outputString = outputString.concat(searches.get(i));
                        outputString = outputString.concat(" ");
                        //System.out.println(searches.get(i));
                    }
                }
                return String.format(outputString);
            }   
        }
        return "404 Not Found!";
    }
}

public class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

## Using the program

Running `SearchEngine` will provide a link to the locally hosted server:

![Image](/runningSearchEngine.png)

Within the `Handler` class, there is a method called `handleRequest` that handles the URLs and interprets the paths to perform the desired task. `handleRequest` takes the URL itself as an argument and returns a String that will be displated on the page.

With the `/add` path, the program handles the query following the `s=` as a String that is to be added to the `searches` ArrayList. Here, I added several strings to `searches`:

![Image](/addfire.png)
![Image](/addburn.png)
![Image](/addfirefight.png)
![Image](/addfirewall.png)
![Image](/addsupahotfire.png)
![Image](/addsupahotfye.png)

The additional function of the page is that I can then search through `searches` to find any String that contains whatever query I search for.

To do this, I will use the `/search` path. The query after the `s=` will be interpreted as a String to be used as an argument to a `.contains()` method for each of the Strings within the `searches` ArrayList.

All of the "matches" will be output on the page. Here, I used "fire" as my search query:

![Image](/searchResults.png)

---

## Week 3: Debugging

During week 3's lab, I worked with JUnit to test and debug several bits of Java code.

In debugging, we want to be careful with our choice of words. It is important to be able to differentiate between "symptoms", and the "bug" itself. While symptoms are how we observe the code to "fail", they are not the "bug" itself. Bugs are the code itself that lead to the failure observed in symptoms. It is also important to note what conditions, or the "failure inducing inputs", are that display the bug in the program.

Here are two examples of the buggy methods I dealt with during the lab:

## Debugging `reversed()`

The specifications of the `reversed()` method was that it would return a new array containing all the elements that the input array held, in the reversed order.

The failure-inducing input: 

```
int[] input1 = { 0, 1, 2, 3, 4, 5 };
assertArrayEquals(new int[]{ 5, 4, 3, 2, 1, 0 }, ArrayExamples.reversed(input1));
```

The symptom:
```
2) testReversed2(ArrayTests)
arrays first differed at element [0]; expected:<5> but was:<0>
```

The bug: The for loop was copying newArray to arr instead of the other way around, and was also returning arr instead of newArray. So I changed the code from:
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
}
```
to
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1]; //array names were reversed
    }
    return newArray; //was returning arr, not newArray
}
```

Conclusion: The bug was that the newArray array was putting its elements (which were the defaulted values initialzed to be 0) into the arr array, leaving newArray unchanged. Not to mention, the method also returned arr, so there was still going to be nothing but zeros in the output, which we can see from the test results (the symptoms). The input illustrated this bug, as it allowed the test to see the difference between the expected reversed array, and the actual zeroed out array.

## Debugging `filter()`

The specifications of the `filter()` method was that it would return a new ArrayList containing only the Strings within the input ArrayList that passed the StringChecker. Additionally, the elements were to be in the *same order as they appeared in the input.*

The failure-inducing input: 

```
List<String> input1 = new ArrayList<String>();
input1.add("The");
input1.add("Quick");
input1.add("Brown");
input1.add("Fox");
input1.add("jumped");
input1.add("over");
input1.add("the");
input1.add("Lazy");
input1.add("Dog.");
List<String> expected = new ArrayList<String>();
expected.add("The");
expected.add("Quick");
expected.add("Brown");
expected.add("Fox");
expected.add("Lazy");
expected.add("Dog.");
assertEquals(expected, ListExamples.filter(input1, new StartsWithCapital()));
```

The symptom:
```
1) testFilter(ListTests)
java.lang.AssertionError: expected:<[The, Quick, Brown, Fox, Lazy, Dog.]> but was:<[Dog., Lazy, Fox, Brown, Quick, The]>
```

The bug: 
```
for(String s: list) {
      if(sc.checkString(s)) {
        result.add(s); // was "result.add(0, s);", leading to the list being put in the reverse order
}
```

Conclusion: Essentially, the bug was that for each String in the input, it would continually place it at the 0 index. This would lead to the sequential pass through the input to add the proceeding element at the front of the output, pushing the preceding elements to the end, resulting in an output reversed from the expected. This is seen in the symptoms, or the test results returning the reverse of our expected result. The input used illustrated that, while the selection of elements were correct, the order was incorrect.
