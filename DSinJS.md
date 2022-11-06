# Map

 - Ordered collection of key value pair.
 - they are itrables.
 
| Objects | Maps  |
|--|--|
| Ordered | Un-ordered  |
| keys --> string or symbol | keys --> string only  |
| It has a prototype which contains some predefined keys | It doesnot contain any predefined keys by dafault  |
| numbers of items can be determined manually | Number of items is available with the size property of map.  |
| Objects can be used to store data as well as funtionality related to that data | Maps are used to store only data.  |

    const map = new Map([
      ['name', 'vikas'],
      ['age', 25],  
    ]);
    
    map.set('company', true);
    
    console.log(map);
    console.log(map.get('age'));
    console.log(map.has('name'));
    map.delete('company')
    
    console.log(map.size);
    
    map.clear()

# Stack

 - Last in - first out
 - It is an abstract data type. It is defined by its behavior rather then being a mathmatical model.
 - Push(element) - Pop - Peek - IsEmpty - Print - Size.
 - 

    class Stack {
      constructor() {
        this.items = [];
      }
    
      show() {
        return [...this.items];
      }
    
      push(val) {
        if (val) {
          this.items.push(val);
          return true;
        }
        return false;
      }
    
      pop() {
        return  this.items.pop();
      }
    
      get isEmpty() {
        return this.items.length === 0;
      }
    
      get peek() {
        return this.items[this.items.length - 1];
      }
    }
    
    let stack = new Stack();

# Queue

 - First in - First out.
 - Abstract data type.
 - Head and tail.
 - Enqueue ---> Add at tail.
 - Dequeue ---> Remove at head.
 - Peek - IsEmpty - Size
 - 

> With better time complexity

    class Queue {
      constructor() {
        this.items = [];
        this.headPointer = -1;
        this.tailPointer = -1;
      }
    
      show() {
        return [...this.items];
      }
    
      enqueue(val) {
        if (val) {
          if (this.isEmpty) {
            this.headPointer = 0;
          }
          this.items.push(val);
          this.tailPointer = this.tailPointer + 1;
          return true;
        }
        return false;
      }
    
      dequeue() {
        let dqElem = this.items[this.headPointer];
        this.items[this.headPointer] = undefined;
        this.headPointer = this.headPointer + 1;    
        return dqElem;
      }
    
      get isEmpty() {
        return this.items[this.tailPointer] === undefined;
      }
    
      get peek() {
        return this.items[this.headPointer];
      }
    }
    
    let queue = new Queue();

> With better space complexity

    class Queue {
      constructor() {
        this.items = [];
      }
    
      show() {
        return [...this.items];
      }
    
      enqueue(val) {
        if (val) {
          this.items.push(val);
          return true;
        }
        return false;
      }
    
      dequeue() {
        return  this.items.shift();
      }
    
      get isEmpty() {
        return this.items.length === 0;
      }
    
      get peek() {
        return this.items[this.items.length - 1];
      }
    }    
    let queue = new Queue();
    

> With better time and space complexity.

    class OptimisedQueue {
      constructor() {
        this.items = {};
        this.tail = 0;
        this.head = 0;
      }
    
      enqueue(elem) {
        if (elem) {
          this.items[this.tail] = elem;
          this.tail++;
          return true;
        }
        return false;
      }
    
      dequeue() {
        if (this.tail > this.head) {
          let item = this.items[this.head];
          delete this.items[this.head];
          this.head++;
          return item;
        }
        return false;
      }
    
      get peek() {
        return this.items[this.head];
      }
    
      get isEmpty() {
        return this.tail === this.head;
      }
    
      get print() {
        return { ...this.items };
      }
    }
    
    const queue = new OptimisedQueue();

## Circular Queue

 - First in - First out
 - Size of queue is fixed.
 - aka circular buffer or ring buffer.
 - enqueue(elem) - dequeue() - isFull() - isEmpty() - peek() - size() - print()
 - 

    class Queue {
      constructor(capasity) {
        this.items = new Array(capasity);
        this.currentLength = 0;
        this.capasity = capasity
        this.rear = -1;
        this.front = -1;
      }
    
      isFull() {
        return this.currentLength === this.capasity;
      }
    
      isEmpty() {
        return this.currentLength === 0;
      }
    
      enqueue(elem) {
        if (!this.isFull()) {      
          this.rear = (this.rear + 1) % this.capasity;      
          this.items[this.rear] = elem;
          if (this.front === -1) {
            this.front = this.rear;
          }
          this.currentLength = this.currentLength + 1;      
        }
      }
    
      dequeue() {
        if (this.isEmpty()) {     
          return null;
        }    
        let elem = this.items[this.front];    
        this.items[this.front] = undefined;    
        this.front = (this.front + 1) % this.capasity;
        if (this.isEmpty()) {
          this.front = -1;
          this.rear = -1;
        }
        this.currentLength = this.currentLength - 1;
        return elem;
      }
    
      peek() {
        return this.items[this.front];
      }
    
      print() {
        let arr = [];
        let i;
        for (i = this.front; i !== this.rear; i = (i + 1) % this.capasity) {
          arr.push(this.items[i]);
        }
        arr.push(this.items[i]);
        return arr;   
      }
    }
    let queue = new Queue(5);

# Linked list

 - linear DS
 - series of connected node.
 - each node contains --> node value and a pointer which points to another node.
 - new node can be added or removed anywhere in linked list,  without any need of reorganising the list.
 - accessing any node has O(n) time complexity. (Array has O(1)).
 - Head pointer points to first node and last node's pointer points to null.
 - if list is empty head pointer points to null.
 - Insertion(elem) -- deletion() -- search()
 - 

    class Node {
      constructor(value) {
        this.value = value;
        this.next = null;
      }
    }
    class LinkedList {
      constructor() {
        this.head = null;
        this.lastNode = null;
        this.size = 0;
        this.items = {};
      }
    
      isEmpty() {
        return this.size === 0;
      } //O(1)
    
      getSize() {
        return this.size;
      } //O(1)
    
      prepend(value) {
        let node = new Node(value);
        if (this.isEmpty()) {
          this.head = node;
          this.lastNode = node;
        } else {
          node.next = this.head;
          this.head = node;
        }
        this.size++;
      } //O(1)
    
      append(value) {
        let node = new Node(value);
        if (this.isEmpty()) {
          this.head = node;
          this.lastNode = node;
        } else {
          this.lastNode.next = node;
          this.lastNode = node;
        }
        this.size++;
      } //O(1)
    
      insert(value, index) {
        if (index < 0 || index > this.size - 1) {
          console.log(`please provide a valid index : Range 0 to ${this.size - 1}`);
          return;
        }
        let node = new Node(value);
    
        if (this.isEmpty() || index === 0) {
          this.prepend(value);
        } else {
          let indexCount = 0;
          for (let i = this.head; indexCount < index; i = i.next) {
            if (indexCount === index - 1) {
              let temp = i.next;
              i.next = node;
              i.next.next = temp;
            }
            indexCount++;
          }
          this.size++;
        }
      } //O(n)
    
      removeByIndex(index) {
        if (index < 0 || index > this.size - 1) {
          console.log(`please provide a valid index : Range 0 to ${this.size - 1}`);
          return null;
        } else if (index === 0) {
          let item = this.head.value;
          this.head = this.head.next;
          this.size--;
          return item;
        } else if (index === this.size - 1) {
          let item = this.lastNode.value;
          for (let i = this.head; i !== null; i = i.next) {
            if (i.next.next === null) {
              this.lastNode = i;
              this.lastNode.next = null;
            }
          }
          this.size--;
          return item;
        } else {
          let indexCount = 0;
          let item;
          for (let i = this.head; indexCount < index; i = i.next) {
            if (indexCount === index - 1) {
              let temp = i.next.next;
              item = i.next.value;
              i.next = temp;
            }
            indexCount++;
          }
          this.size--;
          return item;
        }
      } // O(n)
    
      removeByValue(value) {
        if (this.isEmpty()) {
          return null;
        } else if (value === this.head.value) {
          this.head = this.head.next;
          this.size--;
          return value;
        } else if (value === this.lastNode.value) {
          for (let i = this.head; i !== null; i = i.next) {
            if (i.next.next === null) {
              this.lastNode = i;
              this.lastNode.next = null;
            }
          }
          this.size--;
          return value;
        } else {
          for (let i = this.head; i.next !== null; i = i.next) {
            if (i.next.value === value) {
              i.next = i.next.next;
              this.size--;
              return value;
            }
          }
        }
        return null;
      } //O(n)
    
      searchByValue(value) {
        if (this.isEmpty()) {
          return null;
        }
        let index = 0;
        for (let i = this.head; i !== null; i = i.next) {
          if (i.value === value) {
            return index;
          }
          index++;
        }
        return -1;
      } //O(n)
    
      print() {
        return { ...this.head };
      } //O(n)
    
      get elements() {
        let i;
        let arr = [];
        for (i = this.head; i.next !== null; i = i.next) {
          arr.push(i.value);
        }
        arr.push(i.value);
        return arr;
      } //O(n)     
    }
    
    let list = new LinkedList();

 

> Algorithm to reverse a linked list

1. make 4 variables prev, curr, next
 2. initially prev = null, curr = head   
 3. set next = curr.next
 4. set curr.next = prev
 5. set prev = curr 
 6. set curr = next 
 7. repeat step 3-4-5-6 till curr !== null
 8. set head = prev
 9. 

    reverse() {
        let prev = null;
        let curr = this.head;
        while (curr !== null) {
          let next = curr.next;
          curr.next = prev;
          prev = curr;
          curr = next
        }
        this.head = prev
      } // O(n)
### Making stack with Linked List

> export the Linked list class as a module

    const LinkedList = require('./LinkedList.js');
    
    class StackLinkedList {
      constructor() {
        this.list = new LinkedList();
      }
      push(value) {
        this.list.prepend(value);
      }
    
      pop() {
        return this.list.removeByIndex(0);
      }
    
      peek() {
        return this.list.head.value;
      }
    
      isEmpty() {
        return this.list.isEmpty();
      }
    
      getSize() {
        return this.list.getSize();
      }
    
      print() {
        return this.list.elements;
      }
    }
    
    let stack = new StackLinkedList()

### Making Queue with Linked List

> Export linked list class as a module

    const LinkedList = require('./LinkedList.js');
    
    class QueueLinkedList {
      constructor() {
        this.list = new LinkedList();
      }
      enqueue(value) {
        this.list.append(value);
      }
    
      dequeue() {
        return this.list.removeByIndex(0);
      }
    
      peek() {
        return this.list.head.value;
      }
    
      isEmpty() {
        return this.list.isEmpty();
      }
    
      getSize() {
        return this.list.getSize();
      }
    
      print() {
        return this.list.elements;
      }
    }
    
    let queue = new QueueLinkedList();

## Doubly linked list

 - Unlike linked-list A doubly linked list's node consists of:
		 - previous node
		 - node value
		 - next node
 - Initially :: head = null, tail = null, prev = null, next = null
 - For one node :: head = node, tail = node, prev = null, next = null
# Hash table / Hash map
 - A hash table is a DS that is used to store key-value pairs.
 - Given a key,  we can associate a value with that key for very fast lookup.
 - JS object is a very special implementation of hash table DS. However object class adds it's own keys. Keys that you input may conflict or overwrite the inherited default properties.
 - Like Hash tables there is a Map DS which were introduced in 2015 is also used to store data in key-value pairs.
 - Writing your own hash table implementation is a very popular JS interview question.

> Implementation
- we store key-value pairs in a fixed size array.
- but -- array has a numeric index !
- so, how do we go from using string as an index to number as an index ?
- We use a hashing function.
- Hashing function ===> accepts a string -> converts it into a hash code using a defined logic -> maps it into a numeric index that is within the bounds of the array. --> Store value on that index.
- then same hashing function is used to retrive the value , given a key.
- Operations => get() - set() - remove/delete()

**Adwance programmers may use a complex hashing function but we will use a very basic hashing function for lerning purpose.**

    class HashTable {
      constructor(size) {
        this.table = new Array(size);
        this.size = size;
      }
    
      hash(key) {
        let total = 0;
        for (let i = 0; i < key.length; i++) {
          //Key.charCodeAt(i) returns a numeric value associated with i th character of given Key
          total += key.charCodeAt(i);
        }
        return total % this.size;
      }
    
      set(key, value) {
        let index = this.hash(key);
        this.table[index] = value;
      } // worst:O(1)
    
      get(key) {
        let index = this.hash(key);
        return this.table[index];
      } // worst:O(1)
    
      delete(key) {
        let index = this.hash(key);
        this.table[index] = undefined;
      } // worst:O(1)
    
      display() {
        let str = '{';
        for (let i = 0; i < this.table.length; i++) {
          if (this.table[i]) {
            str += `'${i}':'${this.table[i]}',`;
          }
        }
        str += '}';
        return str;
      } // worst:O(n)
    }
    
    let table = new HashTable(10);
*But in the above hashing function there is a bug ::: key 'name' or 'amen' or 'mane' or 'aenm' or simillar will get stored in the same index and overwrite previous. because our hashing function is very basic and contains this bug. **This bug is known as hash table collision***

**solving hash table collision**

    class HashTable {
      constructor(size) {
        this.table = new Array(size);
        this.size = size;
      }
    
      hash(key) {
        let total = 0;
        for (let i = 0; i < key.length; i++) {
          //Key.charCodeAt(i) returns a numeric value associated with i th character of given Key
          total += key.charCodeAt(i);
        }
        return total % this.size;
      }
    
      set(key, value) {
        let index = this.hash(key);
        let bucket = this.table[index];
        if (!bucket) {
          this.table[index] = [[key, value]];
        } else {
          let sameKeyItem = bucket.find((item) => {
            return item[0] === key;
          });
          if (sameKeyItem) {
            sameKeyItem[1] = value;
          } else {
            bucket.push([key, value]);
          }
        }
      } // worst:O(n), average:O(1)
    
      get(key) {
        let index = this.hash(key);
        let bucket = this.table[index];
        if (bucket) {
          let sameKeyItem = bucket.find((item) => {
            return item[0] === key;
          });
          if (sameKeyItem) {
            return sameKeyItem[1];
          }
        }
        return undefined;
      } // worst:O(n), average:O(1)
    
      delete(key) {
        let index = this.hash(key);
        let bucket = this.table[index];
        if (bucket) {
          let sameKeyItem = bucket.find((item) => {
            return item[0] === key;
          });
          if (sameKeyItem) {
            sameKeyItem[1] = undefined;
          }
        }
      } // worst:O(n), average:O(1)
    
      display() {
        let str = '{';
        for (let i = 0; i < this.table.length; i++) {
          let bucket = this.table[i];
          if (bucket) {
            bucket.forEach((item) => {
              str += `${item[0]} : ${item[1]},`;
            });
          }
        }
        str += '}';
        return str;
      } //O(n2)
    }
    
    let table = new HashTable(10);

# Tree

 - Tree is a heirarichial DS that consists of nodes connected by edges.
 - Tree is a non-linear DS , compared to Stack, Queue, Linked list, Hash tables which are linear DS.
 - In linear DS the time required to search is proportional to the input size. But in non-linear DS that is not the case and it allows quicker and easier search.
 - Tree will not contain any loop or cyles.

### Tree terminologies.
 - Node --> contains data.
 - Edge --> connection bw two node, 
 - Parent node
 - Child node
 - Root node --> Which do no have any parent
 - Leaf node --> Which do not have any child
 - path --> path bw two node (eg. if A-->D-->E then parh(A-E) =  A-D-E)
 - distance --> number of connected edges in path of two nodes.(eg. distance(A-E) = 2)
 - degree of node --> total number of child nodes it has.
 - degree of tree --> max out of the dergree of node of that tree
 - depth of node --> number of connected edges in path of root and that node. (for root, depth = 0)
 - height of node --> number of connected edges in path of  that node and the leaf node associated with it.
## Binary Tree
 - A binary tree is a tree DS in which each node has at most two children known as left and right child.
## Binary serch tree
 - In binary search tree, the value of each left node is less then it's parent node and value of each right node is greater node then its parent node.
 - example --> parent = A; children --> B,C , then value( B ) <value( A ) <value( C )
### BST operations
 - Insertion
 - Search
 - DFS & BFS --> to visit all nodes in the tree.
 - deletion
### BST usage
 - Searching
 - Sorting
 - To implement abstract data-types such as lookup tables and priority queues.
### BST implementation

> Setup

    class Node {
      constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
      }
    }
    
    class BinarySearchTree {
      constructor() {
        this.root = null;
      }
      
      get isEmpty() {
        return this.root === null;
      }     
    }

> insertion : insert(value)

    
      insertNode(root, newNode) {
        if (newNode.value < root.value) {
          if (root.left === null) {
            root.left = newNode;
          } else {
            this.insertNode(root.left, newNode);
          }
        } else {
          if (root.right === null) {
            root.right = newNode;
          } else {
            this.insertNode(root.right, newNode);
          }
        }
      }
    
      insert(value) {
        const newNode = new Node(value);
        if (this.isEmpty) {
          this.root = newNode;
        } else {
          this.insertNode(this.root, newNode);
        }
      }  
    

> Searching : search(value)

    search(value, root = this.root) {
        console.log('seraching');
        if (value === root.value) {
          return true;
        }
        if (value < root.value && root.left) {
          return this.search(value, root.left);
        }
        if (value > root.value && root.right) {
          return this.search(value, root.right);
        }
        return false;
      }

> Tree traversal

 - tree traversal means --> visiting every node in the tree.
 - In linear DS there is only one way to traverse the tree.
 - In tree DS there are two ways -
		 - Depth first serach --> DFS
		 - Breadth first search --> BFS

#### DFS
- DFS traversal starts at the root node and explore as far as possible along each branch before backtracking.
- Visit root node, visit all nodes in left sub tree, visit alll nodes in right sub tree.
- Depending on the order in which we do this , there can be three types of DFS traversal.
		- Pre-order
		- In-order
		- Post-order
##### Pre-order Traversal
*algorithm* 
step1 : read the data of the node
step2 : visit the left sub tree
step3 : visit the right sub tree

    preOrder(root = this.root) {
        if (root) {
          console.log(root.value);
          this.preOrder(root.left);
          this.preOrder(root.right);
        }
    }

##### In-order Traversal : prints values in ascending order
*algorithm* 
step1 : visit the left sub tree
step2 : read the data of the node
step3 : visit the right sub tree

    inOrder(root = this.root) {
        if (root) {
          this.inOrder(root.left);
          console.log(root.value);
          this.inOrder(root.right);
        }
    }
##### Post-order Traversal
*algorithm* 
step1 : visit the left sub tree
step2 : visit the right sub tree
step3 : read the data of the node

    postOrder(root = this.root) {
        if (root) {
          this.postOrder(root.left);
          this.postOrder(root.right);
          console.log(root.value);
        }
    }
#### BFS
- Explore all nodes at the present depth prior to moving on to the nodes at the next depth level.

*algorithm*
step 1 : create a queue
step 2 : enqueue the root node
step 3 : As long as the node exist in the queue perform folowing.

 - dequeue the node from the front
 - read the node value
 - enqueue the node's left child if exists
 - enqueue the nodes right child if exists.
 - 

    bfsTransverse() {
        let queue = [];
        // we can use the optimised queue class also
        if (this.root) {
          queue.push(this.root);
        }
        while (queue.length) {
          let curr = queue.shift();
          console.log(curr.value);
          if (curr.left) {
            queue.push(curr.left);
          }
          if (curr.right) {
            queue.push(curr.right);
          }
        }
    }

> Minimum and Maximum value

    getMin(root = this.root) {
        if(root.left){
          return this.getMin(root.left)
        }
        return root.value
    }
    
    getMax(root = this.root) {
        if(root.right){
          return this.getMax(root.right)
        }
        return root.value
    }


> Deleting a node

there are 3 cases when it comes to deleting a node BST.

 - node doesnt have any child
 - node has a single child
 - node have two children

**case 1 : node doesnt have any child**
- in this case we simply delete the node

**case 2 : node has one child**
- we replace the node with its child

**case 3 : node have two children**
- we set the node's value equat to the min value in right sub tree and repeat delete for that min value in right sub tree. with this the original node which was containing that min value (obously a leaf node) would be deleted.
- 

    delete(value, root = this.root) {
        if (!root || !value) {
          return null;
        }
        if (root.value === value) {
          if (!root.left && !root.right) {
            return null;
          }
          if (root.left && !root.right) {
            return root.left;
          }
          if (!root.left && root.right) {
            return root.right;
          }
          if (root.left && root.right) {
            root.value = this.getMin(root.right);
            root.right = this.delete(root.value, root.right);        
          }      
        }
        if (value < root.value) {
          root.left = this.delete(value, root.left);      
        }
        if (value > root.value) {
          root.right = this.delete(value, root.right);      
        }
        return root;
    }
