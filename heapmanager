#include <stdio.h>
#include <math.h>
#include <stdlib.h>

void my_initialize_heap(int size);
void* my_alloc(int size);
void my_free(void* data);
int roundUp(int numToRound, int multiple);

struct Block
{
    int block_size;           //# of bytes in the data section
    struct Block* next_block; //pointer to next block
    //the data section follows in memory after the above struct
};

//8 bytes
const long strucSize = sizeof(struct Block);
//4 bytes
const long ptrSize = sizeof(void*);
struct Block* free_head;

int main()
{

    my_initialize_heap(600);
    printf("\nsize of a void *: %d", ptrSize);
    printf("\nsize of a overhead: %d", strucSize);

    ////PART 1
    printf("\nPart 1\n");
    int* someInt;
    someInt = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 1 is: %p", (void*)someInt);
    my_free(someInt);
    int* someOtherInt;
    someOtherInt = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 2 is: %p", (void*)someOtherInt);

    ////PART 2
    printf("\nPart 2\n");
    int* someInt2;
    someInt2 = (int*)my_alloc(sizeof(int));
    int* someOtherInt2;
    someOtherInt2 = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 1 is: %p", someInt2);
    printf("\nthe address of int 2 is: %p", someOtherInt2);
    printf("\nThe distance apart is: %ld", (int)someOtherInt2 - (int)someInt2);
    printf("\nThe size of the overhead + the size of a void * is: %d", strucSize + ptrSize);

    ////PART 3
    printf("\n\nPart 3");
    int* someInt3;
    someInt3 = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 1 is: %p", (void*)someInt3);
    int* someOtherInt3;
    someOtherInt3 = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 2 is is: %p", (void*)someOtherInt3);
    int* someOtheOtherInt;
    someOtheOtherInt= (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 3 is is: %p", (void*)someOtheOtherInt);
    my_free(someOtherInt3);
    double* someDouble;
    someDouble = (double*)my_alloc(sizeof(double*));
    printf("\nThe address of double 1 is: %p", (void*)someDouble);

    ////PART 4
    printf("\n\nPart 4");
    char* someChar;
    someChar = (char*)my_alloc(sizeof(char));
    printf("\nthe address of char 1 is: %p", (void*)someChar);
    int* someOtherOtherOtherOtherInt;
    someOtherOtherOtherOtherInt = (int*)my_alloc(sizeof(int));
    printf("\nthe address of int 1 is: %p", (void*)someOtherOtherOtherOtherInt);
    printf("\nThe distance apart is: %d", (int)someOtherOtherOtherOtherInt - (int)someChar);
    printf("\nThe size of the overhead + the size of a void * is: %d", strucSize + ptrSize);

    ////PART 5
    printf("\n\nPART 5");
    int* arrPtr;
    arrPtr = (int*)my_alloc(100 * sizeof(int));
    int* valPtr;
    valPtr = (int*)my_alloc(sizeof(int));
    *valPtr = 8;
    printf("\nThe address of the Int should be 100 * sizeof(int) + valPtr: %p", (100 * sizeof(int) + (valPtr+1)));
    printf("\nThe address of the Int is actually: %p", valPtr);
    my_free(arrPtr);
    printf("\nThe address of the Int is %p",valPtr);
    printf("\nThe value of the Int is %d", *valPtr);

    ////PROGRAM
    int n;
    float sum = 0;
    float summation = 0;
    int* ptr;
    printf("\n\nPROGRAM");
    printf("\nEnter number of elements: ");
    scanf_s("%d", &n);
    ptr = (int*)my_alloc(n * sizeof(int));

    if (ptr == NULL)
    {
        printf("\nNot enough memory to allocate.");
        exit(0);
    }

    for (int i = 0; i < n; ++i)
    {
        printf("Enter element %d :", i+1);
        scanf_s("%d", ptr + i);
        sum += *(ptr + i);
    }

    float mean = sum / n;

    for (int i = 0; i < n; ++i)
    {
        summation += pow(((*(ptr + i)) - mean), 2);

    }

    float calculation = sqrt(summation / n);
    printf("\nThe standard deviation is %.3f", calculation);

}

void my_initialize_heap(int size)
{
    //allocates space for the heap we will use
    free_head = (struct Block*) malloc(size + strucSize);
    //check to see if call to malloc failed
    if (free_head == NULL)
    {
        return;
    }
    //set the values of the block pointed to
    free_head->block_size = size;
    free_head->next_block = NULL;
}

void* my_alloc(int size)
{
    struct Block* previous = NULL;
    struct Block* newFree = NULL;
    struct Block* temp = NULL;
    void* d = NULL;
    int reqSize = 0;

    //checks that no negative numbers are entered
    if (size < 0)
    {
        printf("Size cannot be negative!");
        return NULL;
    }

    //the user requested something that isnt a multiple of the pointer size
    if (size % ptrSize != 0)
    {
        //round it up
        reqSize = roundUp(size, ptrSize);
        size = reqSize;
    }

    //used to traverse the list from the head
    temp = free_head;

    //walk the linked list 
    while (temp != NULL && size > temp->block_size)
    {
        previous = temp;
        temp = temp->next_block;
    }

    //checks if the head is null or the end of the list has been reached
    if (temp == NULL)
    {
        return NULL;
    }

    //split occurs
    if (temp->block_size - size - strucSize > ptrSize)
    {
        d = temp + 1;
        newFree = d + size;
        newFree->block_size = temp->block_size - size - strucSize;
        newFree->next_block = temp->next_block;
        temp->block_size = size;
        temp->next_block = NULL;

        //the split block is the head
        if (temp == free_head || previous == NULL)
        {
            //the new block is the head
            free_head = newFree;
        }
        else
        {
            //the block was not the head, so just add the new block to the list
            previous->next_block = newFree;
        }
    }
    else
    {
        d = temp + 1;

        if (temp == free_head || previous == NULL)
        {
            free_head = temp->next_block;
            temp->next_block = NULL;
        }
        else
        {
            previous->next_block = temp->next_block;
            temp->next_block = NULL;
        }
    }
    //return the pointer back to the user
    return d;
}

void my_free(void* data)
{
    //casting 
    struct Block* temp = (struct Block*)data - 1;
    //place the block at the beginning of the list.
    temp->next_block = free_head;
    //point to this new head
    free_head = temp;
}

//rounds when the requested size is not a multiple of the void * pointer
int roundUp(int numToRound, int multiple)
{
    if (multiple == 0)                             //To handle the base case
    {
        return numToRound;
    }

    //Calculations to round numToRound
    int roundDown = ((int)(numToRound) / multiple) * multiple;
    int roundUp = roundDown + multiple;
    int roundCalc = roundUp;
    return (roundCalc);
}



