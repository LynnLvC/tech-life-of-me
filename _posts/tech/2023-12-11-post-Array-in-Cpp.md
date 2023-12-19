---
title: "Pointer with Array and its name"
toc  : true
categories:
  - tech
  - programming
tags:
  - c++
---
In C++, the array name is the address of the array, just like in C. We could use the array name or the pointer to the array name to index the array.
However, there are differences between a pointer to an array and the array name.

## The pointer
We first define the numeric array and three pointers.
~~~ c++
    int cookies[9] = {1, 2, 4, 8, 16, 32, 64, 128, 512};
    int * pa,*pc;
    pa = cookies;
    pc = &cookies[0];

    int (*pb) [9]; 
    pb = &cookies;

    cout<<"1:\t"  << pa<<"\t"      <<pc<< "\t"<<pb<< endl;
    cout<<"2:\t"  <<*pa<< "\t"     <<*pc<<"\t"<<*pb<< endl;
    cout<<"2-1:\t"<<*(pa+1)<< "\t" <<*(pc+1)<<"\t"<<*(pb+1)<<"\t"<<(*pb)[1]<< endl;
    cout<<"3:\t"  <<sizeof(pa)<<"\t"<<sizeof(pc)<< "\t"<<sizeof(pb)<< "\t"<<sizeof(*pb)<<endl;

    cout<<"4:\t"<< cookies << "\t"<< &cookies << "\t"<< *cookies<< "\t"  << *(&cookies)<<"\t"<<*(&cookies[0])<<endl;
~~~
The output is (on my x64 pc):
~~~
1:      0x5c3d5ffb70    0x5c3d5ffb70    0x5c3d5ffb70
2:      1       1       0x5c3d5ffb70
2-1:    2       2       0x5c3d5ffb94    2
3:      8       8       8       36
4:      0x5c3d5ffb70    0x5c3d5ffb70    1       0x5c3d5ffb70    1
~~~

In the above code, although `pa`,`pb`, and `pc` point to the same address, as shown by the output of **1**,  `pa` and `pc` are the pointer to the first element of the array, just as `&cookies[0]`; `pb`, is the pointer to the array (of size 9).

It looks like that `cookies`, `&cookies`, and `&cookies[0]` point to the same address, however, `&cookies` points to the whole array [^1], just as the pointer `pb`, which is explained elaborately in the next section.
[^1]: C++ Primer Plus, Sixth edition, Stephen Prata, Page 170

## Special case for size of an array

As mentioned above `&cookies` points to the whole array, and `cookies` points to the first element of the array, which can be proved by outputting the array size:
~~~ c++
    cout<<"6:\t"<< *(&cookies + 1) << "\t" << cookies << "\t" << *(&cookies + 1) - cookies<<endl;
    cout<<"7:\t"<<*(&cookies + 1) << "\t" << &cookies[0] << "\t" << *(&cookies + 1) - &cookies[0]<<endl;
    cout<<"6-1:\t"<< *pb << "\t" << *(pb +1) << "\t" << *(pb + 1) - *pb<<endl;
~~~
The output is
~~~
6:      0x5c3d5ffb94    0x5c3d5ffb70    9
7:      0x5c3d5ffb94    0x5c3d5ffb70    9
6-1:    0x5c3d5ffb70    0x5c3d5ffb94    9
~~~
`&cookies` or `pb` points to the address of the array block, so `&cookies +1` or `*(pb+1)` will point to the address after the array. The subtraction will be interpreted to give the array size.

However, what's confusing me is the Output **5** (see the following). `sizeof(cookies)` will give the array size while `sizeof(&cookies)` will give the pointer size (which is 8 in my computer). I cannot find the answer to this, my explanation is that to make it intuitive, the compiler does internal interpretation.

~~~ c++
 cout<<"5:\t"<< sizeof(cookies) << "\t" << sizeof(&cookies) << "\t" << sizeof(cookies[0]) << endl;
//output: 5:      36      8       4
~~~

## Special case for character arrays
Another special thing about the array name is that 
> With `cout` and with most C++ expressions, the name of an array of char, a pointer-tochar,
and a quoted string constant are all interpreted as the address of the first character
of a string.

~~~ c++
char animal[20] = "bear"; // animal holds bear
const char * bird = "wren"; // bird holds address of string
cout << animal << " and "; // display bear
cout << bird << "\n"; // display wren
cout << animal << " at " << (int *) animal << endl;
~~~
Normally, if you give cout a pointer, it prints an address. But if the pointer is type
char *, cout displays the pointed-to string. If you want to see the address of the string, you
have to type cast the pointer to another pointer type, such as int *. [^2]
[^2]: C++ Primer Plus, Sixth edition, Stephen Prata, Page 176.


## Pass array to a function

The function parameter can be declared as an array or a pointer to pass the array to the function. Then inside the function, the array can be indexed as an **array**

---


		
