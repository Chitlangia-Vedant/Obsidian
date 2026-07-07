# [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return _the head of the copied linked list_.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
**Input:** head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
**Output:** [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

```
**Input:** head = [[1,1],[2,1]]
**Output:** [[1,1],[2,1]]
```

**Example 3:**

**![](https://assets.leetcode.com/uploads/2019/12/18/e3.png)**

```
**Input:** head = [[3,null],[3,0],[3,null]]
**Output:** [[3,null],[3,0],[3,null]]
```

**Constraints:**

- `0 <= n <= 1000`
- `-104 <= Node.val <= 104`
- `Node.random` is `null` or is pointing to some node in the linked list.

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        
    }
};
```

# Solution 1

![image](https://assets.leetcode.com/users/just__a__visitor/image_1553084036.png)

- First, the nodes are cloned with random pointers set to **Null**. Meanwhile, the virtual ropes (connecting the original node and their corresponding new node) are created via hashmap.
- Let us say we want to populate the random field of **1**. We first traverse to **3** (top) and then go down to **3** (bot) and set the random field for the bottom 1.
## CODE (Mine)

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        Node *temp=head;
        Node dummy(0);
        Node *temp2=&dummy;
        map<Node *,Node *> mp;
        while(temp!=NULL){
            temp2->next=new Node(0);
            temp2=temp2->next;
            temp2->val=temp->val;
            mp[temp]=temp2;
            temp=temp->next;
        }
        mp[NULL]=NULL;
        temp=head;
        temp2=dummy.next;
        while(temp!=NULL){
            temp2->random=mp[temp->random];
            temp2=temp2->next;
            temp=temp->next;
        }
        return dummy.next;
    }
};
```

# Solution 2
### Step-by-step Explanation

1. **Initialization**:
    
    - Create an empty hash map, `old_to_new`, to store the mapping from old nodes to new nodes.
2. **First Pass - Node Creation**:
    
    - Traverse the original list and for each node, create a corresponding new node.
    - Store the mapping in `old_to_new`.
3. **Second Pass - Setting Pointers**:
    
    - Traverse the original list again.
    - For each node, set its corresponding new node's `next` and `random` pointers based on the hash map.

### Complexity Analysis

- **Time Complexity**: O(n) — Each node is visited twice.
- **Space Complexity**: O(n) — To store the hash map.

## CODE
```cpp
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;
        
        unordered_map<Node*, Node*> old_to_new;
        
        Node* curr = head;
        while (curr) {
            old_to_new[curr] = new Node(curr->val);
            curr = curr->next;
        }
        
        curr = head;
        while (curr) {
            old_to_new[curr]->next = old_to_new[curr->next];
            old_to_new[curr]->random = old_to_new[curr->random];
            curr = curr->next;
        }
        
        return old_to_new[head];
    }
};
```
# Solution 3

The algorithm is divided into 3 steps (**3 passes**) :  
**Let us assume :**  
![image](https://assets.leetcode.com/users/images/3ab58991-e753-4052-9a1a-bfb5030e3819_1612961783.2270477.png)

**STEP 1: PASS 1** _(Altering the link list)_  
Iterate the given link list and at _each_ iteration create a **copy (except random pointer)** of each node and **insert** it just _next to the node it's copied from_ as shown below.

- If **initially** the link list is like : **A->B->C->D**
- Then **after** this step the link list will be : **A->A'->B->B'->C->C'->D->D'**  
    where **A',B',C',D'** nodes are the copy of **A,B,C,D** nodes respectively **except** the **random** pointers.

```ruby
Node* node=head;
while(node){
	Node* temp=node->next;
	node->next=new Node(node->val);
	node->next->next=temp;
	node=temp;
}
```

![image](https://assets.leetcode.com/users/images/726d9160-1c58-4c2b-bbd6-9a61dd427b1f_1612961921.3060675.png)

**STEP 2: PASS 2** _(Copying the random pointers)_  
Again iterate the link list and **alternatively copy** the _old node's random pointer_ (_if exists_) to the _new node's random pointer_ as shown below **`node->next->random=node->random->next`**.

```rust
node=head;
while(node){
	if(node->random)
		node->next->random=node->random->next;
	node=node->next->next; // go to next old node
}
```

![image](https://assets.leetcode.com/users/images/0a5299d4-68ee-4746-8b34-85c944454fa1_1612962505.5484135.png)

**STEP 3 : (PASS 3)** _(Restoring the old link list)_

- Create a **dummy node (here `ans`)** that will be used to **copy the alternative new nodes** from the link list using `helper` node along with **restoring** the _old link list_.
- Finally, return `ans->next` as **`ans`** currenly points to a dummy node as shown below.

```rust
Node* ans=new Node(0); // first node is a dummy node
Node* helper=ans;

while(head){
	// Copy the alternate added nodes from the old linklist
	helper->next=head->next;
	helper=helper->next;

	// Restoring the old linklist, by removing the alternative newly added nodes
	head->next=head->next->next;
	head=head->next; // go to next alternate node   
}
return ans->next; // Since first node is a dummy node
```

![image](https://assets.leetcode.com/users/images/19d05849-6da1-4272-a575-48f4a7e5dad2_1612963056.7245958.png)

## CODE

```rust
class Solution {
public:
    Node* copyRandomList(Node* head) {
        
        // STEP 1: PASS 1
        // Creating a copy (except random pointer) of each old node and insert it next to the old node it's copied from.
        // That is, it will create new alternative nodes which are a copy (except random pointer) of its previous node.
        Node* node=head;
        while(node){
            Node* temp=node->next;
            node->next=new Node(node->val);
            node->next->next=temp;
            node=temp;
        }
        
        // STEP 2: PASS 2
        // Now copy the random pointer (if exists) of the old nodes to their copy new nodes.
        node=head;
        while(node){
            if(node->random)
                node->next->random=node->random->next;
            node=node->next->next; // go to next old node
        }
        
        //STEP 3: PASS 3
        // Copy the alternative nodes into "ans" link list using the "helper" pointer along with restoring the old link list.
        Node* ans=new Node(0); // first node is a dummy node
        Node* helper=ans;
    
        while(head){
            // Copy the alternate added nodes from the old linklist
            helper->next=head->next;
            helper=helper->next;
            
            // Restoring the old linklist, by removing the alternative newly added nodes
            head->next=head->next->next;
            head=head->next; // go to next alternate node   
        }
        return ans->next; // Since first node is a dummy node
    }
};
```

**TIME COMPLEXITY**  
**O(n+n+n)=O(n)** \[ _Each step (pass) takes O(n) time_ }

**SPACE COMPLEXITY**  
**O(1)**