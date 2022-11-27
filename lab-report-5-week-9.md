# Week 9 Lab Report: Auto-Grader

In this week's lab, we will demonstrate an implementation of a webpage auto-grader made with what we've learned in CSE 15L. This includes the use of terminal commands, creation of bash scripts, URL handling, testing, and more.

## The bash script: `grade.sh`

The following is the script used to grade the submissions:

```
# set -e # Gotta comment this out since we want to run past errors

echo ""

CPATH=".;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar"

if ! [[ -e student-submission ]]
then
	mkdir student-submission
else
	rm -rf student-submission
	mkdir student-submission
fi

git clone $1 student-submission

echo ""

if ! [[ -f student-submission/ListExamples.java ]]
then
    	echo "ListExamples.java not found in student submission. 0/4 points."
    	exit
fi

cp TestListExamples.java student-submission/
cp -r lib student-submission/

cd student-submission/

echo "===== COMPILING... ====="
echo ""

javac ListExamples.java > output.txt 2> error.txt
if [[ $? != 0 ]]
then
	cat error.txt
    echo ""
    echo "ListExamples.java did not compile. 0/4 points."
	exit
fi

javac -cp $CPATH *.java > output.txt 2> error.txt
if [[ $? != 0 ]]
then
    cat error.txt
    echo ""
    echo "TestListExamples.java did not compile. It is possible that an issue in ListExamples.java caused the issue. If you find that this is not the case, contact a TA."
    echo "0/4 points."
    exit
fi

echo "===== RUNNING... ====="
echo ""

java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > output.txt 2> error.txt

if grep -q "OK (4 tests)" output.txt
then
    echo "All tests passed! 4/4 points."
else
    cat output.txt
    FAILURES=`grep -c "(TestListExamples)" output.txt`
    PASSES=$((4 - $FAILURES))
    echo "Test(s) failed! $PASSES/4 points."
fi
```

---

## Examples of auto-grader on browser

Here are three examples of running the auto-grader on a browser:

1. Here, we tested the [original starter code](https://github.com/ucsd-cse15l-f22/list-methods-lab3) from lab 3 in the browser. Its bugs led to the symptoms of the filter method reversing the result, as well as the merge method timing out. This led to half of the tests failing, and so the student recieved 2/4 points.

![Image](/originalOnServer.png)

2. Here, we tested the [corrected code](https://github.com/ucsd-cse15l-f22/list-methods-corrected) in the browser. It had no bugs, and thus recieved full credit.

![Image](/correctedOnServer.png)

3. Here, we tested a [repository with more subtle bugs](https://github.com/ucsd-cse15l-f22/list-examples-subtle). The symptom was that merge failed to deal with duplicate elements in the desired fashion. This led to one test failing, and so the student recieved 3/4 points.

![Image](/subtleOnServer.png)

---

## Tracing the script on the original starter code

To illustrate what is going on in further depth, we will trace what occurs in `grade.sh` when testing the original starter code.

Here, we are making use of bash variables in order to facilitate the use of complex paths:

```
CPATH=".;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar"
```

This block of code avoids the duplication of a `student-submission` directory, ensuring that there is only one at a time, and that it is empty and prepared for use in the rest of the script.

In this particular case, we are running the script for the first time, and thus student-submission does not exist, and thus the condition evaluates to true, and the directory `student-submission` is created.

```
if ! [[ -e student-submission ]]
then
	mkdir student-submission
else
	rm -rf student-submission
	mkdir student-submission
fi
```

Now that `student-submission` exists and is empty, we can `git clone` a repository fed into the first command line argument of the bash script. 

In this case, we fed the repo: [https://github.com/ucsd-cse15l-f22/list-methods-corrected](https://github.com/ucsd-cse15l-f22/list-methods-corrected):

```
git clone $1 student-submission

echo ""
```

Here, we now test if `ListExamples.java` is missing. This is to ensure that there is code within the student's repository that we can actually test on, and it is in the desired folder hierarchy. In this case, the submission matches the desired hierarchy, and thus ListExamples.java is found, and the script continues:

```
if ! [[ -f student-submission/ListExamples.java ]]
then
    	echo "ListExamples.java not found in student submission. 0/4 points."
    	exit
fi
```

Now we are copying our test into the `student-submission` directory, as well as it's dependancies. Following that, we change directory to `student-directory`, where we will stay for the rest of the script: 

```
cp TestListExamples.java student-submission/
cp -r lib student-submission/

cd student-submission/
```

Now we are ensuring that we properly handle compilation errors. We display the errors so that the student can possibly debug their issue, and they will recieve 0/4 points. In this case however, there are no issues in compiling, and thus the error code (accessed with `$?`) is 0, and we continue the run:

```
echo "===== COMPILING... ====="
echo ""

javac ListExamples.java > output.txt 2> error.txt
if [[ $? != 0 ]]
then
	cat error.txt
    echo ""
    echo "ListExamples.java did not compile. 0/4 points."
	exit
fi
```

It is also possible that our tests fail to compile, which we can also detect with `$?`. There are several potential reasons for this, and so we make sure to delinate this to the student and they will recieve 0/4 points in the meantime. In this case however, our tests succeed in compiling, and we continue the run:

```
javac -cp $CPATH *.java > output.txt 2> error.txt
if [[ $? != 0 ]]
then
    cat error.txt
    echo ""
    echo "TestListExamples.java did not compile. It is possible that an issue in ListExamples.java caused the issue. If you find that this is not the case, contact a TA."
    echo "0/4 points."
    exit
fi
```

Now, we run the tests. The following conditional lines use `grep` detect text that occur should the test succeed/fail. In this case, some did fail, and thus we did not get the "OK (4 tests)" text in the output. So, we then count for the amount of failures (detected by the appearance of "(TestListExamples)") and subtract the full credit of 4 points from that. In this case, we had two failures, and thus the student recieved 2/4 points:

```
echo "===== RUNNING... ====="
echo ""

java -cp $CPATH org.junit.runner.JUnitCore TestListExamples > output.txt 2> error.txt

if grep -q "OK (4 tests)" output.txt
then
    echo "All tests passed! 4/4 points."
else
    cat output.txt
    FAILURES=`grep -c "(TestListExamples)" output.txt`
    PASSES=$((4 - $FAILURES))
    echo "Test(s) failed! $PASSES/4 points."
fi
```

---

## Conclusion

The creation of this auto-grader not only allowed us to test our skills in what we have learned throughout this course, but also allowed us to gain further insight into several things; the automation of certain processes using scripts, how webpages are formed, and the daily jobs of our CSE 15L TAs!