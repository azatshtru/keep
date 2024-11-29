# Rythspace 0.1 base ideas
### Notes and links   
There are spaces, each space has a root note.   
Every note has a NoteID or hyperalias.   
Every note can create a link to another note inside it by [[NoteID]] or ./NoteID.   
If linked note doesn't exist, it will be created upon entering the linked note, this is (possibly) the standard way to create new notes.   
   
### Tags and queries   
Every note can be tagged using `@`. For example, `@hello`    
Tags can have properties, for example `@due-date: 2024/07/21`  or `@count: 42` or `@hello: world`    
A **query** can be written inside any note by` @List[tag1, tag2, (tag3 > 4), tag4 < 2025/11/23, tag5="hello"`]    
The above query will be stored as plaintext, but during parsing, it will query the notes containing the tags along with the supplied property conditions and display the results of the query in a list view right inside the note containing the query. A calendar view, gallery view can also be used by specifying `@Calendar[]`  or `@Gallery[]` in place of `@List[]`. Other views might be added later.   
A query can be used to manage and find the notes inside root node or other branches all in one place in different views.   
Query results are sorted in order of the supplied tags.   
Queries can be tabulated with tags as columns by enclosing tags in parentheses.   
   
### Possible ideas to handle orphaned notes with no links.   
Try to think about a way to handle a situation that if the note link is removed, how will the user access the orphaned note. Possible ideas are that all notes are shown in some kind of graph or some list somewhere in ryth.   

Move the note to some kind of archive, from there, the note is moved out automatically if it's NoteID referenced somewhere in the base notes in the space.   
#ryth #fleeting-ideas