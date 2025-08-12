# Handebug Structure Editor 

Rules of Engagement 
15 November 1988

To add your own structures to Handebug's structure editor, you must edit the `HANDY:handebug.defs` file using the following format:

```
STRUCTURE
	NUMBER n
	NAME string
	FIELD
		POSITION x,y
		SIZE 8/16
		TYPE HEX/DEC/BIN/TEXT/POINTER/STRUCT n
		TITLE
			POSITION x,y
			TEXT string
```

Every structure definition starts out with the word `STRUCTURE`.

Within a structure you declare the structure's type number and an optional name. Following that you declare one or more `FIELD` entries. The fields must be declared in the order that they are present in the actual structure. Any field can be edited by the user.

Each field has an x,y position, which is the position of the first displayed character of the field, whether that character is a digit or part of a text string. A field has a size, which is currently limited to 8-bit or 16-bit. A field also has a type, which can be any of the types listed below. Note that the `POINTER` type is a 16-bit pointer to data, whereas the `STRUCT` type specifies that the field is a pointer to another structure, a structure of number `n`. If you double-click on a field that is of type `STRUCT` and the field is non-null, the structure editor will load and display the structure pointed to by the `STRUCT` field. Finally, the field can have one or more `TITLE` definitions, which allows you to have one or more text strings rendered with the field. The fact that you can have multiple `TITLES` per field allows you to get as descriptive as you want.

Comments are created by having a semi-colon as the first character of the line.

Here's an example. To get this display: 

```
	Field 1: 134E
             ----
	Field 2: 01101010
```

You could make this structure definition: 

```
STRUCTURE 
	NUMBER 16 
	NAME Test Structure 
	FIELD 
		SIZE 16 
		TYPE HEX 
		POSITION 10,0 
		TITLE
			POSITION 0,0 
			TEXT Field 1:
		TITLE
			POSITION 10,1 
			TEXT ----
	FIELD 
		SIZE 8 
		TYPE BIN 
		POSITION 10,2 
		TITLE 
			POSITION 0,2 
			TEXT Field 2:
```

Forthcoming features:

- You'll be able to define an initial position and size for a structure's window
- You'll be able to specify that you want normal 16-bit hexidecimal values displayed in $high/low order without having to use the kludge of `POINTER` declaration (after all, not all 16-bit hex values are pointers)
- Comments can start anywhere on a line, not just in the first position
