Download Link: https://assignmentchef.com/product/solved-cmpt280-assignment4-heaps
<br>
<h2>1.1    Heaps</h2>

A <em>heap </em>is a binary tree which has the following <em>heap property</em>: the item stored at a node must be at least as large as any of its descendents (if it has any). In a heap, when an item is removed, it is always the largest item (the one stored at the root) that gets removed. Also, the only item that is allowed to be inspected is the top of the heap, in much the same way that the only item of a stack that may be inspected is the top element. Stacks, queues, and heaps are all examples of collections of data items that we call <em>dispensers</em>. You can put stuff into a dispenser, but the user doesn’t get to specify where – the collection decides according to some rule(s). Likewise, you can take something out of a dispenser, but the dispenser decides what item you get. Dispensers maintain a current item using an internal cursor, but the dispenser always decides what is the current item, and thus the item that will next be <em>dispensed </em>when a user asks to remove or inspect the current item. Dispensers do not have public methods to control the cursor position because the user is not supposed to control this; it’s up to the dispenser. In a stack, the “current” item is always the item at the top of the stack. In a queue it is the item at the front of the queue. In a heap it is the item at the root of the heap.

In question 1 you will implement a heap by writing a class called ArrayedHeap280 that extends the abstract class ArrayedBinaryTree280&lt;I&gt; and implements the Dispenser280&lt;I&gt; interface. Here are brief pseudocode sketches of the insert and deleteItem algorithms:

<table width="624">

 <tbody>

  <tr>

   <td colspan="2" width="578">Algoirthm insert(H, e)Inserts the element e into the heap H.Insert e into H normally, as in ArrayedBinaryTreeWithCursors280&lt;I&gt; // (put it in the left-most open position at the bottom level of thewhile e is larger than its parent and is not at the root:swap e with its parent</td>

   <td width="46">tree)</td>

   <td width="4"> </td>

  </tr>

  <tr>

   <td width="4"> </td>

   <td colspan="3" width="624">Algorithm deleteItem(H)Removes the largest element from the heap H.// Since the largest element in a heap is always at the root…Remove the root from H normally, as in ArrayedBinaryTreeWithCursors280&lt;I&gt;// (copy the right-most element in the bottom level, e, into the root,// remove the original copy of e.)while e is smaller than its largest child swap e with its largest child</td>

  </tr>

  <tr>

   <td width="4"></td>

   <td width="571"></td>

   <td width="45"></td>

   <td width="4"></td>

  </tr>

 </tbody>

</table>

<h2>1.2    Efficient Insertion and Deletion in AVL Trees</h2>

For AVL trees, we need to be able to identify critical nodes to determine if a rotation is required. After each recursive call to insert(), we need to check whether the current node is critical (the restoreAVLProperty algorithm). This means we need to know the node’s imbalance, which means we need to know the heights of its subtrees. If we compute the subtree heights with the recursive post-order traversal we saw in class, we are in trouble, because this algorithm costs <em>O</em>(<em>n</em>) time, where <em>n </em>is the number of nodes in the tree. Since insertion requires <em>O</em>(log <em>n</em>) checks for critical nodes, computing imbalance in this way makes insertion <em>O</em>(<em>n </em>log <em>n</em>) in the worst case. To avoid this cost, in each node of the AVL tree we have to store the heights of both of its subtrees, and update these heights locally with each insertion and rotation. The insertion algorithm from the AVL tree slides becomes:

<table width="624">

 <tbody>

  <tr>

   <td width="624">// Recursively insert data into the tree rooted at R Algorithm insert(data, R) data is the element to be insertedR is the root of the tree in which to insert ’data’// This algorithm would only be called after making sure the tree // was non-empty. Insertion into an empty tree is a special case.if data &lt;= R.item() if( R.leftChild == null )R.leftChild = new node containing dataelse insert(data, R.left)recompute R.leftSubtreeHeight // Can be done in constant time!else if( R.rightChild == null )R.rightChild = new node containing dataelse insert(data, R.right)recompute R.rightSubtreeHeight // Can be done in constant time!restoreAVLProperty(R)</td>

  </tr>

 </tbody>

</table>

Observe that if we recurse the left subtree, then when that recursion finishes, we recompute the left subtree height but not the right subtree height (since we know the right subtree didn’t change!). Likewise, if we recurse right, then only the right subtree’s height need be recomputed when the recursive call returns (the left subtree didn’t change!).

As the above algorithm indicates, the key is to recompute subtree heights in constant time. Let <em>p </em>be a node, and let <em>l </em>and <em>r </em>be <em>p</em>’s left and right children, respectively. As long as <em>l </em>and <em>r</em>’s subtree heights are correct, then the subtree heights for <em>p </em>can be recomputed in constant time. For example, if <em>l</em>.leftHeight and <em>l</em>.rightHeight are correct, then we can recompute <em>p</em>’s left subtree height as:

max(<em>l</em>.leftHeight, <em>l</em>.rightHeight)+ 1

Since we are adjusting subtree heights starting from the bottom level of the tree where the new node was inserted, we can correct subtree heights at each level in constant time as the recursion unwinds back up the tree because all children of the current node will already have correct subtree heights.

It is important to note that, in addition to recomputing subtree heights as the recursive calls to insert unwind, it is also necessary to correct subtree heights whenever a rotation occurs. Rotations rearrange nodes, and change the heights of subtrees. For both the left and right rotations, there are exactly two nodes involved for which one subtree height has to be recomputed. Again, this can be done in constant time similar to as above. It will be up to you to figure out the exact details of adjusting the subtree height during rotations. Double rotations should pose no additional problems since they can be written in terms of two single rotations.

<h1>2      Your Tasks</h1>

<strong>Question 1 (10 points):</strong>

Implement a heap by writing a class called ArrayedHeap280 that extends the existing abstract class

ArrayedBinaryTree280&lt;I&gt; and implements the Dispenser280&lt;I&gt; interface. The only methods you should need to write are a constructor, and the insert and deleteItem methods required by Dispenser280&lt;I&gt;, but you can use additional private methods if you think it makes sense to do so. The algorithms for insertion and deletion are given in Section 2.1 of this document.

Note that since you need to compare items in the heap to each other, you must write the generic type parameter of your class’ header so that it is required to be a subclass of Java’s Comparable interface. This is necessary to ensure that only items that implement the Comparable interface can be stored in the heap. As a consequence, you’ll have to make sure that your constructor initializes the instance variable items (inherited from ArrayedBinaryTree280&lt;I&gt;) to an array of Comparable. If this is all done properly, then any item x in the heap can be compared to another item y using x.compareTo(y).

A regression test is provided for you in the heaptests.txt. Copy the two functions therein into your ArrayedHeap280 class. You do not need to write your own test. The test functions will likely exhibit compiler errors if your class does not have the correct class header. Your implementation is highly likely to be correct if it passes the provided regression test.

While you should always practice good commenting habits, we will not be grading javadoc or inline comments on this question.

<strong>Question 2 (53 points):</strong>

Implement an AVL tree ADT. The design of the class is up to you. You may choose to use as much or as little of lib280-asn4 as you desire (including extending existing classes, implementing existing interfaces, taking pieces of code for your own methods, or not using any of lib280-asn4 at all) but you will be graded on these choices, so make good choices.

Regardless of your high-level design decisions, you can earn full marks for correctness of implementation as long as the following requirements are met:

<ul>

 <li>Your AVL tree ADT must be able to contain elements of any comparable object type, but all elements in a single tree are the same type.</li>

 <li>Your ADT must support insertion of new elements, one at a time. Additionally, the insertion algorithm implementation must have the following properties:

  <ul>

   <li>It must be <em>O</em>(log <em>n</em>) in the worst case. This means you have to store subtree heights in the nodes (see previous section) to avoid incurring the linear-time cost of a recursive height method. Part marks will be given for solutions that are at worst <em>O</em>(<em>n </em>log <em>n</em>), but such a solution will receive a major deduction. No marks will be given for solutions where insertion and deletion is worse than <em>O</em>(<em>n </em>log <em>n</em>).</li>

   <li>The tree must restore the AVL property after insertions using (double) left and right rotations. <strong>– </strong>You are free to choose whether or not to allow duplicate items in the AVL tree.</li>

  </ul></li>

 <li>Your ADT must have an operation for determining whether a given element is in the tree.</li>

 <li>Your ADT must have an operation that deletes an element. The mechanism for specifying the item to delete is up to you. Additionally, the deletion algorithm implementation must have the following properties:

  <ul>

   <li>It must be <em>O</em>(log <em>n</em>) in the worst case. Again, part marks will be given for solutions that are at worst <em>O</em>(<em>n </em>log <em>n</em>), but such a solution will receive a major deduction. No marks will be given for solutions where insertion and deletion is worse than <em>O</em>(<em>n </em>log <em>n</em>).</li>

   <li>It must restore the AVL property after deletions using (double) left and right rotations.</li>

  </ul></li>

 <li>You must include a test program (in a main() method) that demonstrates the correctness of your implementation. The purpose of this test program is to demonstrate to the markers that your ADT meets the above requirements. Your program’s output should be designed to convince the marker that your insertion, rotation, and lookup, and search methods work correctly. Note that the goals here are different from a normal “regression test”, so console output even when there are no errors is not only acceptable, but required. For example, you can print out the tree, describe what operation is about to be performed, and then print the resulting tree. <strong>The onus is on you to use your test program to demonstrate that your implementation works.</strong>. Thus you should demonstrate cases that invoke all of the different kinds of rotations and special cases that might arise. Your examples should be non-trivial, but simple enough that they can be easily verified by inspection.</li>

 <li>You may not modify any existing classes in lib280-asn4. But you may make new classes as you see fit.</li>

</ul>

<strong>Hints and Notes</strong>

<ul>

 <li>Start on this question early! It’s not that the solution is particularly difficult, but it requires planning. Make sure you give yourself ample time to attempt it, and ask for help if you get stuck. If you wait until the day before the due date begin, there is a high probability that you will not complete this question. You don’t need to <strong>finish </strong>early, but you should start planning early. Also keep in mind that partial solutions can earn partial marks.</li>

 <li>Your early design choices can have an impact on how hard the implementation is (indeed this is true of <strong>any </strong>non-trivial software project!). Think things through before you begin coding. Sketch out the class architecture (UML diagrams are good for this!), and/or algorithms on paper first.</li>

 <li>Steal the toStringByLevel method (found in LinkedSimpleTree280) and modify it so that prints the left and right subtree heights along with each node’s contents. This will help immensely with debugging because even if you implement insertions and rotations correctly, you’ll get incorrect results if the subtree heights are recorded incorrectly. This is because incorrect subtree heights will trigger rotations when none are actually needed, or prevent necessary rotations from occurring.</li>

 <li>If you choose to use a cursor, your ADT need not have methods that allow the user full control over the cursor position (e.g. goFirst(), goForth(), before(), etc.). That is, your class does not need to implement LinearIterator280&lt;I&gt;.</li>

 <li>Make sure everything else is working correctly before you attempt the delete operation. If you design things well, the delete operations should be able to re-use all of the work you did on rotations for the insertion operation.</li>

</ul>

<strong>Evaluation of Question 2</strong>

<strong>There are marks are allocated to good commenting. This includes both inline comments and Javadoc comments for each method header and instance variable.</strong>

Marks will be awarded based on the quality of the design of your class(es). Your use of modularization, encapsulation, and choices when using or not using protected/private methods will be considered. That said, there is no need to be overly fancy. If appropriate, make use of existing classes in lib280-asn4, but consider your options before you start and think about which of the possible approaches will be easier to implement.

The remaining marks will be for allocated for the correctness (as demonstrated by your test program) and efficiency of the implementation of the insertion, lookup, and deletion operations. The detailed grading rubric can be found on Moodle.

<h1>3      Files Provided</h1>

<strong>lib280-asn4.zip: </strong>Contains the ArrayedBinaryTree280&lt;I&gt; class that you will extend in Question 2. <strong>heaptests.txt: </strong>Functions for testing your ArrayedHeap280&lt;I&gt; implementation.