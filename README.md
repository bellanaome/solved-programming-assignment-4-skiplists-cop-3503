Download Link: https://assignmentchef.com/product/solved-programming-assignment-4-skiplists-cop-3503
<br>
<strong>Abstract</strong>

In this assignment, you will implement probabilistic skip lists. You will gain experience working with a probabilistic data structure and implementing reasonably complex insertion and deletion algorithms. You will also solidify your understanding of generics in Java by making your skip lists capable of holding any type of Comparable object. When you’re finished, you’ll have a very powerful and well-constructed container class.

Don’t be too intimidated by this assignment (unless that’s what it takes to get you to start early, in which case you should be <strong>V</strong><strong>E</strong><strong>R<em>Y</em> I</strong>N<strong>T<em><sub>I</sub></em></strong><strong>M<sub>I</sub></strong><strong>D</strong><strong>A<em>T</em><sub>E</sub>D</strong><strong>!</strong>). This will be a big challenge – probably our most challenging assignment this semester – but it’s very much within your grasp. I’ve already done a lot of the heavy lifting for you by giving you the basic structure of the program and creating lots of test cases, so you get to focus on the fun part, which is taking your knowledge of how the insertion, deletion, and search algorithms for skip lists work, and translating them into code.

<strong>Deliverables</strong>

SkipList.java

<h1>1.      Preliminaries</h1>

In this assignment, you will implement the probabilistic skip list data structure. It is important that you implement the insertion, deletion, and search operations <a href="https://webcourses.ucf.edu/courses/1268970/pages/skip-lists">as described in class</a><a href="https://webcourses.ucf.edu/courses/1268970/pages/skip-lists">.</a> Also, your skip list class must be generic. Since there is an ordering property in skip lists, you must also restrict the type parameter (e.g., AnyType) to classes that implement Comparable.

<h1>2.      Node and SkipList: Multiple Class Definitions in One Source File</h1>

You will have to implement a Node class, but since you are only submitting one source file via Webcourses (SkipList.java), you must tuck the Node class into SkipList.java. That means the Node class cannot be public. I have provided a sample SkipList.java file that demonstrates how to structure your code so that it will work with my test cases.

<h1>3.      Node Class</h1>

<h2>3.1.       Method and Class Requirements</h2>

Implement the following methods in a class named Node. You may replace AnyType with something else, if you wish (e.g., T), as long as the method names and return types stay the same.

<strong>Node</strong>(<strong>int</strong> height)

This constructor creates a new node with the specified height, which will be greater than zero. Initially, all of the node’s <em>next</em> references should be initialized to <em>null</em>. This constructor will be particularly useful when creating a head node, which does not store anything meaningful in its <em>data</em> field. This constructor may be useful to you in other ways as well, depending how you choose to implement certain of your skip list methods.

<strong>Node</strong>(AnyType data, <strong>int</strong> height)

This constructor creates a new node with the specified height, which will be greater than zero, and initializes the node’s value to <em>data</em>. Initially, all of the node’s <em>next</em> references should be initialized to <em>null</em>.

<strong>public </strong>AnyType value();

An O(1) method that returns the value stored at this node.

<strong>public int</strong> height();

An O(1) method that returns the height of this node.

For example, if a node has three references (numbered 0 through 2), the height of that node is 3 (even if some of those references are <em>null</em>).

<strong>public </strong>Node&lt;AnyType&gt; next(<strong>int</strong> level);

An O(1) method that returns a reference to the next node in the skip list at this particular level. Levels are numbered 0 through (<em>height</em> – 1), from bottom to top.

If <em>level</em> is less than 0 or greater than (<em>height</em> – 1), this method should return <em>null</em>.

<h2>3.2.       Suggested Methods</h2>

I found the following methods helpful in implementing my skip lists. You might find them helpful as well, but you aren’t required to implement them. You may also choose to implement these suggested methods with different return types or parameter types. You’re not bound by these method signatures in the same way that you’re bound by the required method signatures above.    <strong>public void</strong> setNext(<strong>int</strong> level, Node&lt;AnyType&gt; node);

Set the <em>next</em> reference at the given level within this node to <em>node</em>.

<strong>public void</strong> grow();

Grow this node by exactly one level. (I.e., add a <em>null</em> reference to the top of its tower of <em>next</em> references). This is useful for forcing the skip list’s <em>head</em> node to grow when inserting into the skip list causes the list’s maximum height to increase.

<strong>public void</strong> maybeGrow();

Grow this node by exactly one level with a probability of 50%. (I.e., add a <em>null</em> reference to the top of its tower of <em>next</em> references). This is useful for when inserting into the skip list causes the list’s maximum height to increase.

<strong>public void</strong> trim(<strong>int</strong> height);

Remove references from the top of this node’s tower of <em>next</em> references until this node’s height has been reduced to the value given in the <em>height</em> parameter. This is useful for when deleting from the skip list causes the list’s maximum height to decrease.

<h1>4.      SkipList Class</h1>

<h2>4.1.       A Few Notes about the Head Node</h2>

The height of your skip list’s <em>head</em> node should almost always be log⌈         <sub>2</sub>n (the ceiling of log⌉  <sub>2</sub>n), where <em>n</em> is the number of elements in the skip list (i.e., the number of nodes in the skip list, excluding the <em>head</em> node). Recall that a skip list’s <em>head</em> node does not contain an actual value, and therefore does not count as a node in your skip list when calling the <em>size()</em> method described below.

There are three exceptions to this rule for the height of the <em>head</em> node:

<ol>

 <li>A skip list that has exactly one element should have a height of 1 (not log⌈ <sub>2</sub>(1)⌉, which is 0).</li>

 <li>If the skip list contains no elements, you may set the height of the <em>head</em> node to either 0 or 1. That choice is yours. You should pick the value that plays nicely with however you’re implementing the other methods in your SkipList class.</li>

 <li>One of the SkipList constructors described below allows the programmer to manually set the height of the <em>head</em> node when the skip list is first created. This is useful if you know that you’re going to insert enough elements to cause the skip list to grow several times. In that case, you might as well start off with the skip list’s height being greater than 1, even though there are no elements in the skip list to begin with. (This behavior is clarified in a number of test cases included with this assignment.)</li>

</ol>

Throughout this document, the height of the <em>head</em> node is considered to be synonymous with the height of your skip list.

<h2>4.2.       Method and Class Requirements</h2>

Implement the following methods in a class named SkipList. You may replace AnyType with something else if you wish (e.g., T), as long as the function names and return types stay the same.

SkipList();

This constructor creates a new skip list. The height of the skip list is initialized to either 0 or 1, as you see fit. (See note about the height of the <em>head</em> node, above.)

SkipList(<strong>int</strong> height);

This constructor creates a new skip list and initializes the <em>head</em> node to have the height specified by the <em>height</em> parameter. If the height is less than the default height you have chosen for empty lists (0 or 1), you may instead default to your chosen value of 0 or 1.

The skip list should remain this tall until either it contains so many elements that the height must grow (i.e., when log⌈  <sub>2</sub>n⌉ exceeds the given height of the skip list), or when an element is successfully deleted from the skip list. For further details, see the <em>insert()</em> and <em>delete()</em> method descriptions, below.

<strong>public int </strong>size();

In O(1) time, return the number of nodes in the skip list (excluding the head node, since it does not contain a value).

<strong>public int</strong> height();

In O(1) time, return the current height of the skip list, which is also the height of the <em>head</em> node. Note that this might be greater than log⌈   <sub>2</sub>n if the skip list was initialized with a height greater ⌉ than 1 using the second constructor listed above.

<strong>public </strong>Node&lt;AnyType&gt; head();

Return the head of the skip list.

<strong>public void</strong> insert(AnyType data);

Insert <em>data</em> into the skip list with an expected (average-case) runtime of O(log n). If the value being inserted already appears in the skip list, this new copy should be inserted <em>before</em> the first occurrence of that value in the skip list. (See test cases #14 and #15 for further clarification on the order in which duplicate values should be inserted.)

You will have to generate a random height for this node in a way that respects both the maximum height of the skip list and the expected distribution of node heights. (I.e., there should be a 50% chance that the new node has a height of 1, a 25% chance that it has a height of 2, and so on, up to the maximum height allowable for this skip list.)

The maximum possible height of this new node should be either log⌈         <sub>2</sub>n⌉ or the current height of the skip list – whichever is greater. (Recall that the height of the skip list can exceed log⌈ <sub>2</sub>n⌉ if the list was initialized using the second SkipList constructor described above.)

If inserting this node causes log⌈        <sub>2</sub>n to exceed the skip list’s current height, you must increase ⌉ the overall height of the skip list. Recall that our procedure for growing the skip list is as follows: (1) the height of the <em>head</em> node increases by 1, and (2) each node whose height was maxed out now has a 50% chance of seeing its height increased by 1. For example, if the height of the skip list is increasing from 4 to 5, the head node must grow to height 5, and each other node of height 4 has (independently) a 50% chance of growing to height 5.

Test Cases #1 through #3 demonstrate how the <em>insert()</em> method should affect the height of a skip list under a variety of circumstances. Test Cases #4 and #5 speak to the expected height distribution of nodes in a skip list.

<strong>public void</strong> insert(AnyType data, <strong>int</strong> height);

Insert <em>data</em> into the skip list using the procedure described above, but do not generate a random height for the new node. Instead, set the node’s height to the value passed in via this method’s <em>height</em> parameter. This will be super handy for testing your code with your own sequence of not-so-random values and node heights.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>   <strong>public void</strong> delete(AnyType data);

Delete a single occurrence of <em>data</em> from the skip list (if it is present). If there are multiple copies of <em>data</em> in the skip list, delete the first node (that is, the leftmost node) that contains <em>data</em>. The expected runtime for this method should be O(log n), so you cannot perform a linear search for this node along the bottom-most level of node references. You must use the search algorithm for skip lists described in class.

If this method call results in the deletion of a node, and if the resulting number of elements in the list, <em>n</em>, causes log⌈  <sub>2</sub>n to fall below the current height of the skip list, you should trim the ⌉ maximum height of the skip list to log⌈   <sub>2</sub>n . When doing so, all nodes that exceed the new ⌉ maximum height should simply be trimmed down to the new maximum height.

If this method call does not actually result in a node being deleted (i.e., if <em>data </em>is not present in the skip list), you should <em>not</em> modify the height of the skip list.

Test Cases #8 through #11 and Test Case #13 clarify how the <em>delete()</em> method should affect the height of a skip list under a variety of circumstances.

<strong>public boolean</strong> contains(AnyType data);

Return <em>true</em> if the skip list contains <em>data</em>. Otherwise, return <em>false</em>. The expected (average-case) runtime of this function must be O(log n), and you must use the search algorithm for skip lists described in class. Test Case #6 demonstrates how the runtime of this method might be tested.

<strong>public </strong>Node&lt;AnyType&gt; get(AnyType data);

Return a reference to a node in the skip list that contains <em>data</em>. If no such node exists, return <em>null</em>. If multiple such nodes exist, return the first such node that would be found by a properly implemented <em>contains()</em> method.

<strong>   public static double</strong> difficultyRating()

Return a double on the range 1.0 (ridiculously easy) through 5.0 (insanely difficult).

<strong>   public static double</strong> hoursSpent()

Return an estimate (greater than zero) of the number of hours you spent on this assignment.

<h2>4.3.       Suggested Methods</h2>

I found the following methods helpful in implementing my skip lists. You might find them helpful as well, but you aren’t required to implement them. You may also choose to implement these suggested methods with different return types or parameter types. You’re not bound by these method signatures in the same way that you’re bound by the required method signatures above. <strong>   private static int</strong> getMaxHeight(int n)

A method that returns the max height of a skip list with <em>n</em> nodes.

<strong>   private static int </strong>generateRandomHeight(int maxHeight)

Returns 1 with 50% probability, 2 with 25% probability, 3 with 12.5% probability, and so on, without exceeding <em>maxHeight</em>.

<strong>   private void</strong> growSkipList()

Grow the skip list using the procedure described above for the <em>insert()</em> method.

<strong>   private void</strong> trimSkipList()

Trim the skip list using the procedure described above for the <em>delete()</em> method.

<h1>5.      Output</h1>

None of the methods described above should print anything to the screen. Printing anything to the screen will likely resolve in tragic test case failure.

<h1>6.      Compiling and Testing Your SkipList Class on Eustis</h1>

Recall that your code must compile, run, and produce precisely the correct output on Eustis in order to receive full credit. Here’s how to make that happen:

<ol>

 <li>To compile your program with one of my test cases:</li>

</ol>

javac SkipList.java TestCase01.java

<ol start="2">

 <li>To run this test case and redirect your output to a text file:</li>

</ol>

java TestCase01 &gt; myoutput01.txt

<ol start="3">

 <li>To compare your program’s output against the sample output file I’ve provided for this test case:</li>

</ol>

diff myoutput01.txt sample_output/TestCase01-output.txt

If the contents of myoutput01.txt and TestCase01-output.txt are exactly the same, diff won’t print anything to the screen. It will just look like this:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="245741454a575e64415157504d57">[email protected]</a>:~$ diff myoutput01.txt sample_output/TestCase01-output.txt  <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="681b0d09061b12280d1d1b1c011b">[email protected]</a>:~$ _

Otherwise, if the files differ, diff will spit out some information about the lines that aren’t the same.

<h1>7.      Using the test-all.sh Script</h1>

I’ve also included a script called test-all.sh that you can use to run all the test cases I released with this assignment. You can run it on Eustis by placing it in a directory with SkipList.java, all the test case files, and the sample_output subdirectory, and typing:

bash test-all.sh

<strong>Note!</strong> Because Test Case #6 generates a lot of data, you might not be able to run it on Eustis in a reasonable amount of time. It’s totally acceptable to make sure everything is compiling and running on Eustis, but to then use your own personal computer to test the actual runtime of Test Case #6.

<h1>8.      Grading Criteria and Miscellaneous Requirements</h1>

The <em>tentative</em> scoring breakdown (not set in stone) for this programming assignment is:

<table width="586">

 <tbody>

  <tr>

   <td width="57">80%</td>

   <td width="529">program compiles and passes test cases (which might include checks that generics with Comparable are properly implemented and that your program compiles without warnings and without manually suppressing Xlint:unchecked warnings)</td>

  </tr>

  <tr>

   <td width="57">10%</td>

   <td width="529">difficultyRating() and hoursSpent() return doubles in the specified ranges</td>

  </tr>

  <tr>

   <td width="57">10%</td>

   <td width="529">adequate comments and whitespace; source code includes student name</td>

  </tr>

 </tbody>

</table>

Please be sure to submit your .java file, not a .class file (and certainly not a .doc or .pdf file). Your best bet is to submit your program in advance of the deadline, then download the source code from Webcourses, re-compile, and re-test your code in order to ensure that you uploaded the correct version of your source code.

<strong>Important! Programs that do not compile will receive zero credit. </strong>When testing your code, you should ensure that you place SkipList.java alone in a directory with the test case files (source files and sample_output directory), and no other files. That will help ensure that your SkipList.java is not relying on external support classes that you’ve written in separate .java files but won’t be including with your program submission.

<strong>Important! You might want to remove </strong><strong>main() and then double check that your program compiles without it before submitting.</strong> Including a main() method can cause compilation issues if it includes references to home-brewed classes that you are not submitting with the assignment. Please remove.

<strong>Important! Your program should not print anything to the screen. </strong>Extraneous output is disruptive to the grading process and will result in severe point deductions. Please do not print to the screen.

<strong>Important! Please do not create a java </strong><strong>package. </strong> Articulating a package in your source code could prevent it from compiling with our test cases, resulting in severe point deductions.

<strong>Important! Name your source file, class(es), and method(s) correctly.</strong> Minor errors in spelling and/or capitalization could be hugely disruptive to the grading process and may result in severe point deductions. Similarly, failing to write any one of the required methods, or failing to make them public and/or static (as appropriate) may cause test case failure. Please double check your work!

<strong>Input specifications are a contract.</strong> We promise that we will work within the confines of the problem statement when creating the test cases that we’ll use for grading. Please reflect carefully on the kinds of edge cases that might cause unusual behaviors for any of the methods you’re implementing.

<strong>You should create additional test cases.</strong> Please be aware that the test cases included with this assignment writeup are by no means comprehensive, and serve as a bare minimum to get you started with testing your code. Please be sure to create your own test cases and thoroughly test your code. Sharing test cases with other students is allowed, but you should challenge yourself to think of edge cases before reading other students’ test cases.

<em>Start early! Work hard! Ask questions! Good luck!</em>

<a href="#_ftnref1" name="_ftn1">[1]</a> The <em>height</em> parameter passed to this method will always be on the range [1, log⌈             <sub>2</sub>(n + 1) ], where ⌉ <em>n</em> is the number of nodes in the skip list before adding this new node. This accounts for the fact that the new node (the +1 in that statement) might cause the height of the skip list to grow. For example, if the current height is 3 and the new element would cause the height to grow to 4, the valid range for that height parameter is 1 through 4.