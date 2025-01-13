---
course_name: java programming
trimester_week: 4
---

## type conversions   
- widening conversions are taken care by the Java compiler   
- narrowing conversion needs to be done manually in the c way   
- truncation happens in float to int where decimals drop   
- type promotion happens when some types are promoted to other types, for example, if either operand is a double, then the other operand in the operation is also promoted to double, if either operand is a float, then other operand is converted to float, smaller integers are converted to higher integers. for example, integers are converted to long if either operand is a long.   
   
### errors due to type promotion   
```
byte b = 50;
b = b * 2;
```
the above code will throw an error because in second line, the byte b is multiplied to integer 2, which returns an integer, which we cannot assign back to the byte b.   
proper casting needs to be done to assign this   
```
byte b = 50;
b = (byte)(b * 2);
```
   
### arrays   
To declare an array, the syntax is:   
```
type array[];
```
to define an array of some size, the syntax is:   
```
array = new type[n]
```
here type is any datatype in java, array is the name of array variable and n is the size of array.   
   
everytime an array is redefined, the older values of array are treated as garbage and cleared by the garbage collector and new space of size n is assigned to it.   
   
arrays can be conveniently declared and defined in one statement using the syntax:   
```
type array[n];
```
this defines an array of size n.   
   
values can be inserted into array at the time of definition using the syntax:   
```
type array[] = { v1, v2, ... }
```
