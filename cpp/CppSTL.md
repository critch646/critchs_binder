# C++ STL

Provides existing templated stacks, queues, lists, etc.

Many of these are for collections of data items and we often want to iterate across all the elements in a collection.

The STL provides a mechanism that is common to all of them: iterator.

```
List<int> L;  // L is a list of ints.
List<int>::iterator it; // variable to refer to currently accessed element. An iterator for a list of ints.

it = L.begin(); // Set the iterator to refer to the first item in the collection

// While not off end, print current, move iterator to next
while (it != L.end()){
	int current;
	current = *it; //copy currently referenced item into current
	cout << current;
	it++; // ++ or -- moves forward or back
}

```