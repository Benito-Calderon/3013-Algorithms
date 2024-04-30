#
//Benito Calderon
//3013-Algorithms
#include <iostream>
#include <fstream>
using namespace std;
class Vector
{
private:
    int *array;
    int capacity;
    int size;

public:
    // Constructors
    Vector(int *A, int size)
{
        capacity = size;
        this->size = size;
        array = new int[capacity];
        for (int i = 0; i < size; i++)
        {
            array[i] = A[i];
        }
}

    Vector(const string& fileName) 
{
        ifstream file(fileName);
        if (!file.is_open()) 
        {
            cout << "Error opening file: " << fileName << endl;
            return;
        }
        capacity = 10; // initial capacity
        size = 0;
        array = new int[capacity];
        int val;
        while (file >> val) 
        {
            pushRear(val);
        }
        file.close();
    }

    Vector(const Vector& other) 
{
        capacity = other.capacity;
        size = other.size;
        array = new int[capacity];
        for (int i = 0; i < size; i++)
        {
            array[i] = other.array[i];
        }
    }

    // Destructor
    ~Vector()
    {
        delete[] array;
    }

    // Utility functions
    void pushFront(int val)
    {
        if (size == capacity)
        {
            // Resize array if needed
            resize();
        }
        for (int i = size; i > 0; i--)
        {
            array[i] = array[i - 1];
        }
        array[0] = val;
        size++;
    }
    //push front function
    void pushFront(const Vector& other) 
    {
        for (int i = other.size - 1; i >= 0; i--)
        {
            pushFront(other.array[i]);
        }
    }
    //push rear function
    void pushRear(int val) 
    {
        if (size == capacity)
        {
            // Resize array if needed
            resize();
        }
        array[size] = val;
        size++;
    }

    void pushRear(const Vector& other) 
    {
        for (int i = 0; i < other.size; i++)
        {
            pushRear(other.array[i]);
        }
    }

    void pushAt(int loc, int val) 
    {
        if (loc >= 0 && loc < size) 
        {
            if (size == capacity) 
            {
                // Resize array if needed
                resize();
            }
            for (int i = size; i > loc; i--) 
            {
                array[i] = array[i - 1];
            }
            array[loc] = val;
            size++;
        }
    }

    void inOrderPush(int val) 
    {
        // Assumes the array is sorted in ascending order
        int i = size - 1;
        while (i >= 0 && array[i] > val)
        {
            array[i + 1] = array[i];
            i--;
        }
        array[i + 1] = val;
        size++;
    }

    int popFront() 
    {
        //exception handling here
        if (size == 0) {
            cout << "Vector is empty!" << endl;
            return -1;
        }
        int val = array[0];
        for (int i = 0; i < size - 1; i++)
          {
            array[i] = array[i + 1];
        }
        size--;
        return val;
    }

    int popRear() {
        if (size == 0)
        {
          //more exception handling
            cout << "Vector is empty!" << endl;
            return -1; 
        }
        int val = array[size - 1];
        size--;
        return val;
    }

    int popAt(int loc)
    {
        if (loc >= 0 && loc < size)
        {
            int val = array[loc];
            for (int i = loc; i < size - 1; i++)
            {
                array[i] = array[i + 1];
            }
            size--;
            return val;
        }   
        else 
        {
          //exception handling
            cout << "Invalid index!" << endl;
            return -1; 
        }
    }

    int find(int val) 
{
        for (int i = 0; i < size; i++) 
        {
            if (array[i] == val) {
                return i;
            }
        }
        return -1; // Not found
    }

    void print()
   {
        cout << "[";
        for (int i = 0; i < size; i++)
        {
            cout << array[i];
            if (i < size - 1) 
            {
                cout << ", ";
            }
        }
        cout << "]" << endl;
    }

private:
    void resize()
    {
        capacity *= 2;
        int *temp = new int[capacity];
        for (int i = 0; i < size; i++) 
        {
            temp[i] = array[i];
        }
        delete[] array;
        array = temp;
    }
};

int main()
{
    // Example usage
    int arr[] = {1, 2, 3};
    Vector v1(arr, 3);
    v1.print(); // [1, 2, 3]

    Vector v2("data.txt");
    v2.print(); // prints the contents of the file

    Vector v3(v1);
    v3.print(); // [1, 2, 3]

    v3.pushFront(0);
    v3.print(); // [0, 1, 2, 3]

    v3.pushRear(4);
    v3.print(); // [0, 1, 2, 3, 4]

    return 0;
}
