Read [[Method vs Function]]

**Value receiver** makes a copy of the type and pass it to the function. The function stack now holds an equal object but at a different location on memory. That means any changes done on the passed object will remain local to the method. The original object will remain unchanged.

**Pointer receiver** passes the address of a type to the function. The function stack has a reference to the original object. So any modifications on the passed object will modify the original object.

