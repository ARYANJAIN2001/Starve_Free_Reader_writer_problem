# Starve_Free_Reader_writer_problem
### Semaphore
```cpp
// The code for a FIFO semaphore.
struct Semaphore{
  int value = 1;
  FIFO_Queue* Q = new FIFO_Queue();
}
    
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
struct ProcessBlock
{
    ProcessBlock* next;
    int* process_block;
}
```
### Intialization
```cpp
int read_count = 0;                      //Integer representing the number of reader executing critical section
Semaphore turn = new Semaphore();        //seamphore representing the order in which the writer and 
                                         //reader are requesting access to critical section
Semaphore rwt = new Semaphore();         //seamphore required to access the critical section
Semaphore r_mutex = new Semaphore();     //seamphore required to change the read_count variable
```
### Reader's Code
```cpp
do{
/* ENTRY SECTION */
       wait(turn,process_id);              //waiting for its turn to get executed
       wait(r_mutex,process_id);           //requesting access to change read_count
       read_count++;                       //update the number of readers trying to access critical section 
       if(read_count==1)                   // if I am the first reader then request access to critical section
         wait(rwt);                        //requesting  access to the critical section for readers
       signal(turn);                       //releasing turn so that the next reader or writer can take the token
                                             and can be serviced
       signal(r_mutex);                    //release access to the read_count
/* CRITICAL SECTION */
       
/* EXIT SECTION */
       wait(r_mutex,process_id)            //requesting access to change read_count         
       read_count--;                       //a reader has finished executing critical section so read_count decrease by 1
       if(read_count==0)                   //if all the reader have finished executing their critical section
        signal(rwt);                       //releasing access to critical section for next reader or writer
       signal(r_mutex);                    //release access to the read_count  
       
/* REMAINDER SECTION */
       
}while(true);
```
### Writer's code
```cpp
do{
/* ENTRY SECTION */
      wait(turn,process_id);              //waiting for its turn to get executed
      wait(rwt,process_id);               //requesting  access to the critical section
      signal(turn,process_id);            //releasing turn so that the next reader or writer can take the token
                                          //and can be serviced
/* CRITICAL SECTION */

/* EXIT SECTION */
      signal(rwt)                         //releasing access to critical section for next reader or writer

/* REMAINDER SECTION */

}while(true);
```
