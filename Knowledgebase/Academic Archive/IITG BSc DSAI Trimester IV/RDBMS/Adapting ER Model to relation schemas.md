---
course: rdbms
week: 6
---
#rdbms

## :LiListChecks: Basic adaptation rules
While adapting the ER model to relation schemas, the following rules are followed:
1. The strong entity sets are converted to relations as is with all their attributes. 
2. For the weak entity sets, other than adding all the discriminator partial attributes the required attributes that complete their primary key are also added to the corresponding relation.
3. Relationship sets contain primary keys of both the entities they connect, they also contain descriptive attributes. Primary key of the relationship set is assigned according to the rules of one-to-many, many-to-one, one-to-one, and many-to-many as discussed before.
4. Composite attributes are modelled using their sub-attributes. For example, if there is a composite attribute containing three sub attributes, then those three are added to the corresponding relation in place of the composite attribute. If it gets complex, then it can be normalized into a separate relation.
5. Derived attributes are not taken into the corresponding relation.
6. Multivalued attributes can be represented using another separate relation.
## Considerations while adapting ER Model to relation schemas
1. Relationships and entities are modeled into separate schemas, no two entity schemas should share the same attributes, rather, entities and relationships schemas share some common attributes that are used to connect two entity schemas via relationship schemas.
2. Relationship schemas shouldn't contain primary keys of their own that are not taken from the primary keys of the entities they connect.
3. If a relationship contains multivalued attributes, then either a composite relational attribute must be used, or a weak entity must be used.

 The third consideration needs some explaining.
	As an example, take two entities: `student` and `section` that are connected by a relationship `stud_section`. 
	The `stud_section` relationship schema contains another two attributes: `assignment` and `marks`.
	The problem is that there can be multiple marks for an assignment, hence we need to turn `marks` attribute into a composite attribute.
	Another way we can solve this is by using weak entity sets, we can create a new weak entity set called `assignment`, this will contain just the `assignment_id`.
	Then we can replace the `stud_section` relationship relation with two new relationship relations:
	1.  a `marks_in` relation that contains `student_ID`, a `marks` attribute and an `assignment_id`.
	2.  a `sec_assign` weak relation that will connect the assignment to the `section` relation to identify the assignments uniquely.
	Then we can connect `student` to `assignment` via `marks_in` and `assignment` to `section` via `sec_assign`.
### Some general guidelines, none set in stone
1. Take attributes out of relations and form new entity sets if they get too complex. Connect the newly formed entity sets to the original relation with a relationship relation.
2. Relationships should be made such that they describe verbs. A relationship between two entities should represent an action one entity is doing on the other.
## Redundancy of relationship schemas for one-to-many and many-to-one relationships
For many-to-one, and one-to-many relationships, an extra attribute can be added to the "many" side that contains the primary key of the "one" side. This effectively connects the two relations without requiring a relationship schema between them.
