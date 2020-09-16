# Binary Trees

## What are Trees

* When you think of a tree the most common occurance that we see on a daily basis is our file directories, where we have subdirectires, and files in a tree like structure.
* A binary search tree is a binary tree with one more restriction: All children to the left of a node have smaller values, whereas all children to the right will have larger values.

![](images/image01.png)

* If a tree is balanced the average search time is directly proportialn to its height: O(h). In the above example the longes path from the root node to a leaf is three, so you can expect the average search time of O(3).
* Remeber that constants don't matter so the average seach time could be reduced to O(1).

## Minimum

* The minimum of a binary search tree is the node with the smallest value, and, thanks to the properties of

## Maximum

* Whereas the minimum of a binary search tree is the node with the smallest value, the maximum is the node

## Successor

![](images/image01.png)

* A node’s successor is the one with the next largest value in the tree. For example, given the tree shown:
	* The successor of A is D
	* The successor of H is I
	* The successor of I is K. 
* Finding the successor is not that difficult, but it involves two distinct cases.

## Predecessor

![](images/image01.png)

* The predecessor of a node is the one with the next smallest value. For example, the predecessor of P is M,

## Seaching

* When searching for a value in a binary search tree, you start at the root node and follow links left or
	* Start at the root node.
	* If there is no current node, the search value was not found and you are done. Otherwise, proceed to step 3.
	* Compare the search value with the key for the current node.
	* If the keys are equal, then you have found the search key and are done. Otherwise, proceed to step 5.
	* If the search key sorts lower than the key for the current node, then follow the left link and go to step 2.
	* Otherwise, the search key must sort higher than the key for the current node, so follow the right link and go to step 2.

### Seaching example 
* In the below example it shows how you would search for the letter K in the binary search tree.
* Starting with the root node (step 1), you compare your search value, K, with the letter I

	![](images/image02.png)

* Because K comes before I, you follow the right link (step 6), which leads you to the node containing the letter L 

![](images/image03.png)

* You still don’t have a match, but because K is smaller than L, you follow the left link (step 5).

![](images/image04.png)

* Finally, you have a match: The search value is the same as the value at the current node (step 4), and your search completes. You searched a tree containing nine values and found the one you were looking for in only three comparisons. In addition, note that you found the match three levels

## Insertion

* Insertion is nearly identical to searching except that when the value doesn’t exist, it is added to the tree

![](images/image05.png)

* The newly inserted value was added as a leaf, which in this case hasn’t affected the height of the tree.
* Inserting relatively random data usually enables the tree to maintain its O(log N) height, but what happens
* Can you imagine what would happen if you started with an empty tree and inserted the following

![](images/image06.png)

* Considering that new values are always inserted as leaf nodes, and remembering that all larger values

# Balancing

* The order in which data is inserted into and deleted from binary search trees affects the performance. More specifically, inserting and deleting ordered data can cause the tree to become unbalanced and, in

* One of the most difficult tasks in maintaining a balanced tree is detecting an imbalance in the first place. Imagine a tree with hundreds if not thousands of nodes. How, after performing a deletion or insertion,

* Two Russian mathematicians, G. M. Adel’son-Vel’skii and E. M. Landis (hence the name AVL), realized that one very simple way to keep a binary search tree balanced is to track the height of each subtree. If two siblings ever differ in height by more than one, the tree has become unbalanced.

* Here is a tree that needs rebalancing. Notice that the root node’s children differ in height by two.

![](images/image07.png)

* After an imbalance has been detected, you need to correct it, but how? The solution involves rotating

|  Inbalanced | Child Is Balanced  | Child Is Left-Heavy  | Child Is Right-Heavy  | 
|---|---|---|---|
| Left-Heavy  | Once   | Once   | Twice   |
| Right-Heavy  | Once  | Twice   | Once  |

# The Unit Test 

![](images/image01.png)


The NodeTest class defines some instance variables—one for each node shown above and initializes them in setUp() for use by the test cases. The first four nodes are all leaf nodes and as such need only the value. The remaining nodes all have left and/or right children, which are passed in as the second and third constructor parameters, respectively: 

```
@Before
public void setUp(){
    _a = new Node("A");
    _h = new Node("H");
    _k = new Node("K");
    _p = new Node("P");
    _f = new Node("F", null, _h);
    _m = new Node("M", null, _p);
    _d = new Node("D", _a, _f);
    _l = new Node("L", _k, _m);
    _i = new Node("I", _d, _l);
}

```

The design calls for the **minimum()** and **maximum()** methods (among others) to be part of the Node class.


```
@Test
public void testMinimum() {
    assertSame(_a, _a.minimum());
    assertSame(_a, _d.minimum());
    assertSame(_f, _f.minimum());
    assertSame(_h, _h.minimum());
    assertSame(_a, _i.minimum());
    assertSame(_k, _k.minimum());
    assertSame(_k, _l.minimum());
    assertSame(_m, _m.minimum());
    assertSame(_p, _p.minimum());
}

@Test
public void testMaximum() {
    assertSame(_a, _a.maximum());
    assertSame(_h, _d.maximum());
    assertSame(_h, _f.maximum());
    assertSame(_h, _h.maximum());
    assertSame(_p, _i.maximum());
    assertSame(_k, _k.maximum());
    assertSame(_p, _l.maximum());
    assertSame(_p, _m.maximum());
    assertSame(_p, _p.maximum());
}
```

Next are successor() and predecessor(). Again, you put these methods on Node, rather than have them as utility methods in BinarySearchTree.


```
@Test
public void testSuccessor() {
    assertSame(_d, _a.successor());
    assertSame(_f, _d.successor());
    assertSame(_h, _f.successor());
    assertSame(_i, _h.successor());
    assertSame(_k, _i.successor());
    assertSame(_l, _k.successor());
    assertSame(_m, _l.successor());
    assertSame(_p, _m.successor());
    assertNull(_p.successor());
}

@Test
public void testPredecessor() {
    assertNull(_a.predecessor());
    assertSame(_a, _d.predecessor());
    assertSame(_d, _f.predecessor());
    assertSame(_f, _h.predecessor());
    assertSame(_h, _i.predecessor());
    assertSame(_i, _k.predecessor());
    assertSame(_k, _l.predecessor());
    assertSame(_l, _m.predecessor());
    assertSame(_m, _p.predecessor());
}
```

You also create another pair of tests—testIsSmaller() and testIsLarger()—for methods that

```
public void testIsSmaller() {
    assertTrue(_a.isSmaller());
    assertTrue(_d.isSmaller());
    assertFalse(_f.isSmaller());
    assertFalse(_h.isSmaller());
    assertFalse(_i.isSmaller());
    assertTrue(_k.isSmaller());
    assertFalse(_l.isSmaller());
    assertFalse(_m.isSmaller());
    assertFalse(_p.isSmaller());
}

@Test
public void testIsLarger() {
    assertFalse(_a.isLarger());
    assertFalse(_d.isLarger());
    assertTrue(_f.isLarger());
    assertTrue(_h.isLarger());
    assertFalse(_i.isLarger());
    assertFalse(_k.isLarger());
    assertTrue(_l.isLarger());
    assertTrue(_m.isLarger());
    assertTrue(_p.isLarger());
}
```

Finally, you create some tests for equals(). The equals() method will be very important when it comes


```
@Test
public void testEquals() {
    Node a = new Node("A");
    Node h = new Node("H");
    Node k = new Node("K");
    Node p = new Node("P");
    Node f = new Node("F", null, h);
    Node m = new Node("M", null, p);
    Node d = new Node("D", a, f);
    Node l = new Node("L", k, m);
    Node i = new Node("I", d, l);
    assertEquals(a, _a);
    assertEquals(d, _d);
    assertEquals(f, _f);
    assertEquals(h, _h);
    assertEquals(i, _i);
    assertEquals(k, _k);
    assertEquals(l, _l);
    assertEquals(m, _m);
    assertEquals(p, _p);
    assertFalse(_i.equals(null));
    assertFalse(_f.equals(_d));
}
```

## Performance

* Performance Comparison for 1,000 Inserts into a Binary Search Tree

| Insertion Type  | Comparisons*  | 
|---|---|
| Random Insertion  | 11,624  |
| In-order Insertion  | 499,500   |