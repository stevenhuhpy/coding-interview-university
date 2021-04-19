- Implement a vector (mutable array with automatic resizing):
    - Practice coding using arrays and pointers, and pointer math to jump to an index instead of using indexing.
    - New raw data array with allocated memory
        - can allocate int array under the hood, just not use its features
        - start with 16, or if starting number is greater, use power of 2 - 16, 32, 64, 128
    - size() - number of items
    - capacity() - number of items it can hold
    - is_empty()
    - at(index) - returns item at given index, blows up if index out of bounds
    - push(item)
    - insert(index, item) - inserts item at index, shifts that index's valueand trailing elements to the right
    - prepend(item) - can use insert above at index 0
    - pop() - remove from end, return value
    - delete(index) - delete item at index, shifting all trailing elementsleft
    - remove(item) - looks for value and removes index holding it (even if inmultiple places)
    - find(item) - looks for value and returns first index with that value,-1 if not found
    - resize(new_capacity) // private function
        - when you reach capacity, resize to double the size
        - when popping an item, if size is 1/4 of capacity, resize to half
    - Time
        - O(1) to add/remove at end (amortized for allocations for more space), index, or update
        - O(n) to insert/remove elsewhere
    - Space
        - contiguous in memory, so proximity helps performance
        - space needed = (array capacity, which is >= n) * size of item, but even if 2n, still O(n)

---

```cpp


class Vector
{
public:
    void Vector(int capacity)
    {
       if (capacity < 1) {                                                <- !!! Missing !!!    
           std::cout << "Cannot make vector of that size" << std::endl;   <- !!! Missing !!!    
           exit(EXIT_FAILURE);                                            <- !!! Missing !!!                        
       }                                                                  <- !!! Missing !!!            

        //!!! should limit by a min capacity, for example the inputs is 1. then the array is mostly will be resized.
        array = new int[capacity];
        vecCapacity = capacity;
    }

    int Size()
    {
        return size;
    }

    int capacity()
    {
        return vecCapacity;
    }

    bool is_empty()
    {
        return size == 0;
    }

    int at(int index)
    {
        ValidateIndex();

        return *(array + index);
    }

    int push(int item)
    {
        LargeCapacity();

        *(array+size) = item;
        size +=1;
    }

    void insert(int index, int item)
    {
        //equals push if index == size
        {
            if (index == size)
            {
                push(item);
            }
        }

        ValidateIndex();

        LargeCapacity();


        for(int i = size; i > index;i++)// !!! should be i--
        {
            *(array+i) = *(array + i -1);
        }

        *(array + index) = item;

        //!!! size ++ ?
    }

    void prepend(int item)
    {
        insert(0, item);
    }

    int pop()
    {
        //!!! what if size ==0 ?;
        delete(size-1);
        if(4 * size <= capacity)
        {
            resize(capacity/2);
        }
    }

    void delete(int index)
    {
        ValidateIndex();

        LargeCapacity();

        for(int i = index; i < size - 1; i++)
        {
            *(array + i) = *(array + i + 1);
        }

        size -= 1;
    }

    ///!!! reuse delete() ?
    void remove(int item)
    {
        int *indexArray = new int[size];
        int sizeOfIndexArray = 0;

        for(int i = 0; i < size; i++)
        {
            if(*(array+i) == item)
            {
                *(indexArray+sizeOfIndexArray) = i;
                sizeOfIndexArray++；
            }
        }

        int *newArray = new int[capacity];
        int j = 0;
        bool findIndex = false;

        for(int i = 0; i< size;i++)
        {
            findIndex = false;
            for(int k = 0; k < sizeOfIndexArray; k++)
            {
                int vecIndex = *(indexArray+k);
                if( i == vecIndex)
                {
                   findIndex = true;
                }
            }

            if(findIndex)
            {
                break;
            }
            else
            {
                *(newArray+j) = *(array+i);
                j++;
            }
        }

        delete indexArray;
        delete array;
        array = newArray;
        
    }

    int find(item)
    {
        for(int i = 0; i < size; i++)
        {
            if(*(array+i) == item)
            {
                return i;
            }
        }

        return -1;
    }

    ///!!! how about case for decreaseSize?
    void resize(int new_capacity)
    {
        if(new_capacity < size)
        {
            throw exception;
        }

        if(capacity == new_capacity)
        {
            return;
        }

        int *temp = new int[2*capcacity];

        for(int i = 0; i < size; i++)
        {
            *(temp+i) = *(array+i);
            
        }

        delete array;
        array = temp;
    }

private:
    void ValidateIndex()
    {
        if(index <0 || index > size)
        {
            throw exception;
        }
    }

    void LargeCapacity()
    {
        if(size == capacity)
        {
            int *temp = new int[2*capcacity];
            for(int i = 0; i < size; i++)
            {
                *(temp+i) = *(array+i);
               
            }
            
            delete array;
            array = temp;
        }
    }

    int *array;
    int size;
    int vecCapacity;
}

```