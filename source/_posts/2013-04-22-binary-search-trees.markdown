---
layout: post
title: "Binary Search Trees"
date: 2013-04-22 14:13
comments: true
categories: 
---

<h2>Properties</h2>
<ol>
	<li>The left subtree of a node contains only nodes with keys less than the node's key.</li>
	<li>The right subtree of a node contains only nodes with keys greater than the node's key.</li>
	<li>The left and right subtree must each also be a binary search tree.</li>
	<li>There must be no duplicate nodes.</li>
</ol>

<h3>Benefits</h3>
<ul>
	<li>Insert, find and remove any entry</li>
	<li>Quickly find entry with min/max key</li>
	<li>Entry nearest another entry, like in a dictionary, but cannot remember how to spell the word</li>
</ul>

<h3>BST Invariant</h3>
<p>For any node x, every key in the left subtree of x is <= x's key</p>
<p>For any node x, every key in the right subtree of x is >= x's key</p>
<p><img src = "/images/BST.png" height = "500" width = "500"></p><br>
<p>Inorder traversal of a binary search tree visits nodes in sorted order, the left of root (18) are displayed before the right.<p/>

<h3>Operations</h3>
<h4>Entry Find (Object k)</h4>
<ul>
	<li>Object looking for compared to object stored in key (if it is there)</li>
	<li>If true, returns the entry</li>
	<li>Else returns nil</li>
	<li>Good for exact matches, can do faster with hash table</li>
</ul><br>
<p>How to find smallest key >= k or largest key <= k?<p/>
	<p>When searching down the tree for a key k that is not in the tree, we encounter both.</p>
	<ul>
		<li>Node containing smallest key > k, and</li>
		<li>Node containing largest key < k </li>
	</ul><br>

	<p>Ex. search for 27 as k (from above pic)</p>
	<ul>
		<li>Keeps searching nodes to the right, gets to 28 and returns nil</li>
		<li>encounters 25 < k and 28 > k</li>
	</ul>

<h4> Entry first();<br> -If empty return null, otherwise start at root and walk down left repeatedly until you reach node with no left child, that node has minimum key.</h4>
<h4> Entry last();<br> -Mirror of left, but walks down right tree</h4>

<h4>Entry insert(Object k, Object v);</h4>
<ul>
	<li>Follow same path through the tree as find();</li>
	<li>When you reach null reference, replace null with reference to new node with entry(k, v), duplicate keys are allowed.</li>
	<li>Puts new entry on the left subtree of old one</li>
</ul>

<h4>The delete operations (where shit gets complex)</h4>
<h4>Entry remove(Object k);</h4>
<ul>
	<li>Find node n with key k, if k not found return null</li>
	<li>If n has no children, detach it from parent</li>
	<li>If n has one child, move child up to take n's place</li>
	<li>If node has 2 children</li>
	<ul>
		<li>Suppose we want to remove 12 from above pic</li>
		<li>Let x be the node in n's right subtree with smallest key</li>
		<ul>
			<li>Keep going to the left, in this case, 13 is node x, only has one child</li>
			<ul>
				<li>Remove x, has no left child and therefore, is easily removed</li>
				<li>Replace n's key with x's key</li>
			</ul>
		</ul>
	</ul>
</ul>

<p><img src = "/images/bstexample.png" height = "500" width = "500"></p><br>

<p>h/t Jonathan Shewchuk at UC Berkeley</p>
<a href='https://www.youtube.com/watch?v=V_3BM0ykITM'>His lecture via Youtube</a> 











