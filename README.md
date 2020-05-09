# Coding Questions
## LRU
```java
class Pair{
    int key;
    int value;
    public Pair(int key,int value){
        this.key = key;
        this.value = value;
    }
}

class Node{
    Pair data;
    Node next;
    Node prev;
    public Node(Pair data){
        this.data = data;
    }
}
class DL{
    Node head;
    Node tail;
    
    public Node deleteFromBeg(){
        if(head == null){
            return null;
        }
        Node temp = head;
        head = head.next;
        temp.next = null;
        head.prev = null;
        return temp;
    }
    public Node deleteFromEnd(){
        if(head == null){
            return null;
        }
        Node temp = tail;
        tail = tail.prev;
        temp.prev = null;
        tail.next = null;
        return temp;
    }
    public Node deleteNode(Node node){
        if(node==head){
            return deleteFromBeg();
        }
        if(node==tail){
            return deleteFromEnd();
        }
        node.prev.next = node.next;
        node.next.prev = node.prev;
        return node;
        
    }
    
    public void insertAtEnd(Node node){
       
        if(head == null){
            head = tail = node;
        }
        node.prev = tail;
        tail.next= node;
        tail = node;
    }
    
}
class LRUCache {

    HashMap<Integer,Node> map = null; 
    DL q = null;
    int cap = 0;
    public LRUCache(int capacity) {
        map = new HashMap<>(capacity);
        q = new DL();
        cap = capacity;
    }
    
    public int get(int key) {
        Node temp = map.get(key);
        
        if(temp == null){
            return -1;
        }
        
        q.deleteNode(temp);
        q.insertAtEnd(temp);
        return temp.data.value;
        
    }
    
    public void put(int key, int value) {
        Node val = map.get(key);
        if(val!=null){
            q.deleteNode(val);
            val.data.value = value;
            q.insertAtEnd(val);            
        }else{            
            if(map.size()==cap)
            {
                Node rem = q.deleteFromBeg();
                map.remove(rem.data.key);
                
                
            }
            Node temp = new Node(new Pair(key,value));
            map.put(key,temp);
            q.insertAtEnd(temp);
            
        }
        
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
 ```

