# Starve_Free_Reader_writer_problem
### Semaphore
```cpp
// The code for a FIFO semaphore.
struct Semaphore
{
    int value = 1;
    Queue* Q = new Queue();
    
    void wait(int process_id)
    {
        value--;
        if(value < 0)
        {
            Q->push(process_id);
            block(); //this function will block the proccess until it's woken up.
            // I have used non-busy waiting but busy waiting is also possible 
            // in case of block() and wakeup() equivalents being not callable or available in the language.
        }
    }
    
    void signal()
    {
        value++;
        if(value <= 0)
        {
            int pid = Q->pop();
            wakeup(pid); //this function will wakeup the process with the given pid.
        }
    }
}

//The code for the queue which will allow us to make a FIFO semaphore.
struct Queue
{
    Node* Front, Rear;
   	void push(int val)
    {
        Node* n = new Node();
        n->value = val;
        if(Rear != NULL)
        {
            Rear->next = n;
            Rear = n;
        }
        else
        {
            Front = Rear = n;
        }
    }
    
    int pop()
    {
        if(Front == NULL)
        {
            return -1; // Error : underflow.
        }
        else
        {
            int val = Front->value;
            Front = Front->next;
            if(Front == NULL)
            {
                Rear = NULL;
            }
            return val;
        }
    }
}

// A queue node.
Struct Node
{
    Node* next;
    int value;
}
```
