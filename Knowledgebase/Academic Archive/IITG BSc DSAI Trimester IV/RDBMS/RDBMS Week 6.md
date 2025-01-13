---
course_name: rdbms
trimester_week: 6
---

# Database Design   
The process of defining relation schemas, constraints and programs to facilitate a database application.   
This is often complex because many parties are involved with different requirements.   
### Database Design is performed in Four Phases   
1. In Phase 1, software design diagrams are created.   
2. In Phase 2, conceptual design model, like ER model is chosen and specifications of transactions and concurrent access are set.   
3. Phase 3 contains logical design of relation schemas and normal forms to avoid redundancy and inconsistency.   
4. Phase 4 is the physical design phase with File Organization, Indexing, Hashing.   
   
# ER Model   
This contains entity sets, relationship sets and attributes.   
## Entities   
An entity is any object, an entity set is a collection of entities of same type with same properties. Entity sets are similar to relations and are defined in the same way R(A1, A2, ... , An)   
A primary key is that property of an entity in a set which uniquely identifies the entity.   
In ER diagrams, entities are represented inside rectangles with entity name on top and list of attributes written inside the rectangle, the primary key is underlined.   
## Relationships   
A relationship is an association among several entities. A relationship set contains relationships of the same kind.   
In ER diagrams, we represent relationships with lines between the entity sets and relationship sets with diamonds between entity rectangles.   
Mathematically, a relationship set is a mathematical cartesian set among n entities, where each entity is taken from an entity set:   

$$
\text{relationship set} =\{(e_1,e_2,\dots,e_n)\vert e_1\in E_1,e_2\in E_2,\dots,e_n\in E_n\}
$$
here $e\_1…e\_n$ are the entities and $E\_1…E\_n$ are the entity sets. This says that the cartesian set among entities taken from different (or same) entity sets is a relationship set. And the component $(e\_1,e\_2,…,e\_n)$ is a relationship.   
In all a relationship is written as (entity1, entity2, …)   
### Entity Roles in relationships   
Entity sets of a relationship need not be distinct. The function that an entity plays in a relationship is called its role.   
### Descriptive attributes in relationships   
A descriptive attribute can be assigned to a relation, this attribute is not a part of any specific relation, rather it describes some property of the relation. The descriptive attribute is written in a box that connected to the relationship diamond via a dotted line.   
### Degree of relationship set   
A relationship can have more than two entities, the number of entities is called the degree of relationship set.   
## Attributes   
The properties associated to entities, they are written after the entity name in the ER diagrams.   
### Attribute types   
- There are simple attributes which hold one value, for example last\_name   
- A composite attribute is made up of several simple attributes, for example:   
   
```
name
	first_name
	middle_name
	last_name

```
- A multivalued attribute contains multiple values, in ER diagrams, it is represented by enclosing the attribute name between braces. `{ phone\_numbers }` A customer entity might have multiple phone numbers.   
- Derived attributes are computed using existing attributes, for example, if we have a date of birth attribute, we can compute an age attribute using the DOB. These are represented by putting parentheses after their names. `age()`    
   
### Domain or Value set   
The set of permitted values for each attribute, for example, a seasons attribute might take values from summer, winter, spring, fall, etc. A numerical age attribute might take values from the set of positive integers.   
## Mapping Cardinality   
For a binary relationship of degree two, if each entity from one set is associated with just one entity from other set to which no other entity from the first set associates to, then it is called a one-to-one relationship.   
each entity can be associated to many entities in the other set, many entities can be associated to just one entity from the other set, or many entities from one set can be associated to many entities in the other set   
We get 4 types of binary relationships.   
1. one-to-one   
2. one-to-many   
3. many-to-one   
4. many-to-many   
   
The idea of "one" is represented as an arrow coming from the relationship diamond to the entity rectangle, the idea of "many" can be represented as a line between relationship diamond and entity rectangle.   
The relationships can also be represented as the notation `l..h` written on the line connecting relationship diamonds and entity rectangles. `l`  and `h` can have values, `0,1,\*` . `0` means partial participation, `1` means full participation and `\*` means "many".   
To represent that each entity from the entity set can associate with only one entity from the other entity set, we write `1..1` on top of the line connecting the rectangle and the diamond. To convey that some or none entities from one set can be associated with many entities from the other set, we write `0..\*` on top of the line connecting the entity rectangle and the relationship diamond.   
### Total or Partial Participation   
In total participation, Every entity in the entity set participates in at least one relationship in the relationship set. In partial participation, some entities do not participate in any relationships in the relationship set.   
Double line in the ER diagram between relationship diamond and entity rectangle represent total participation and single line represent partial participation.   
### Primary Key for Entity sets   
For an entity set, the set of attributes that can distinguish one entity from that set with other entities is called the primary key.   
### Primary Key for Relationship sets   
The primary key for a relationship set depends on the mapping cardinality of the relationship set.   
1. many-to-many: the union of primary keys is a minimal superkey.   
2. one-to-many and many-to-one: the primary key of the "many" side is a minimal superkey.   
3. one-to-one: the primary key of either one of the participating entity sets forms a minimal superkey.   
   
## Redundant attributes   
If two entities in a relationship contain the same attributes, then that attribute is redundant, the redundant attribute couples up the entities unnecessarily when converting the ER model to the relational schemas, we need to remove the redundant attribute to preserve loose coupling.   
The relationships translate into "verbs as relations" as discussed quite a while ago.   
### Weak Entity sets   
Consider that we have two entity sets section(**sec\_id, course\_id, semester, year**) and course(**course\_id**, title, credits). If we want to create a relationship set **sec\_course** between section entity set and course entity set, then we will have a problem of redundancy as both section and course contain a course\_id attribute.   
If we remove the course\_id attribute from section to fix the redundancy problem, then we won't be able to identify a section uniquely. The section entity set will need extra information from the sec\_course relation to uniquely identify its attributes.   
This type of entity set is called a **weak entity set**. Weak entity sets do not have primary key in them. Represented by double lined rectangle.   
The relationship that is required by a weak entity set to uniquely identify its entities is called the ***identifying relation***. Represented by double lined diamond.   
The attributes in the weak entity sets that form the partial key which requires the identifying relation are called the ***discriminator attributes***. Represented by a dashed underline in the weak entity set rectangle.   
   
