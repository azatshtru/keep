---
course_name: rdbms
trimester_week: 3
---

## Concatenation using \|\|   
```
select first_name || last_name from customer
```
This will concatenate the first name and the last name, but without spaces, to get spaces, use a `+`  operator instead of `\|\|`    
```
select first_name + ' ' + last_name from customer
```
 another better way is to use the `concat` function:   
```
select concat(first_name, ' ', last_name) from customer
```
## Extracting substring   
To extract substring from a column of type char(n) or varchar(n), one can use the `substring` function   
for example, to extract the substring from second character to fifth character in a product\_name column, one can write:   
```
select substring(product_name, 2, 5) from product
```
## Case conversion    
`select upper(A1) from somewhere`    
similarly, select lower   
## String Length   
Length of strings of column A1 is found by:   
`select length(A1) from r`    
   
