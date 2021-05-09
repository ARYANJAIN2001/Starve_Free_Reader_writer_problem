# Starve_Free_Reader_writer_problem
### Semaphore
```cpp
// The code for a FIFO semaphore.
typedef Struct{
  int value = 1;
  FIFO_Queue* Q = new FIFO_Queue();
}Semaphore;
    
void wait(Semaphore *S,int* process_id){
  S->value--;
  if(S->value < 0){
  S->Q->push(process_id);
  block(); //this function will block the process by using system call and will transfer it to the waiting queue
           //the process will remain in the waiting queue till it is waken up by the wakeup() system calls
           //this is a type of non busy waiting
  }
}
    
void signal(Semaphore *S){
  S->value++;
  if(S->value <= 0){
  int* PID = S->Q->pop();
  wakeup(PID); //this function will wakeup the process with the given pid using system calls
  }
}


//The code for the queue which will allow us to make a FIFO semaphore.
struct FIFO_Queue{
    ProcessBlock* front, rear;
    int* pop()
    {
        if(front == NULL)
        {
            return -1;            // Error : underflow.
        }
        else
        {
            int* val = front->value;
            front = front->next;
            if(front == NULL)
            {
                rear = NULL;
            }
            return val;
        }
    }
    void* push(int* val){
        ProcessBlock* blk = new ProcessBlock();
        blk->value = val;
        if(rear == NULL){
            front = rear = n;
            
        }
        else
        {
            rear->next = blk;
            rear = blk;
        }
    }
    
}

// A Process Block.
Struct ProcessBlock
{
    ProcessBlock* next;
    int* process_block;
}
```
