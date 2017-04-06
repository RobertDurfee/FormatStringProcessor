# FormatStringProcessor
C++ class for parsing and filling format strings according to user-defined routines and tags.

### Disclaimer
This is not production-worthy code! View this simply as a proof-of-concept. Preconditions are implicit. No error checking exists.

### Initialization
```C++
FormatStringProcessor(void * definedTagFunctionsClass, int numberOfDefinedTags, char ** definedTags, void(**definedTagFunctions)(void * definedTagFunctionsClass, FormatTag * formatTag, char * tagFunctionInput));
```
A `FormatStringProcessor` can be constructed in only one way.

The user must first define a class that contains the static routines that will be linked to each tag. A pointer to this object created by the class must be passed as the first parameter. 

The number of tags that have routines must be passed as the second parameter. This number should include a general routine that will execute if a tag doesn't have a routine.

The tag string specifiers must be passed as the third parameter. The first tag string specifier must be `GENERAL`. These tag string specifiers are case-sensitive.

An array of function pointers representing the routines attached to tag string specifiers must be passed as the fourth parameter. They sould be in the same order as the tag string specifiers. Once again, the first function pointer should point to a general function. Remember, all functions must be `static` methods of the object defined in the first parameter. Especially if the tag specifier routines act on data included in this object.

Data is not copied into this object from parameters, only pointers. As a result, make sure that the parameters passed exist during the entire lifetime of the `FormatStringProcessor` object.

### Resolve
```C++
char * Resolve(char * formatString);
```
A format string is passed as the only parameter in this method. A completed format string is returned by following the routines supplied in the constructor. This completed format string must be freed by calling the standard `free` function.

Each tag in a format string must follow the following example:

`{TAG_ONE{TAG_TWO:{PARAMETER_ONE},{TAG_THREE:{PARAMETER_TWO};};}}`

Note: Tag specifiers can be nested as parameters.
Note: Tag specifiers return results recursively. That is, the result of `TAG_ONE` is passed into `TAG_TWO` as `formatTagInput`.
Note: Whitespace is, unfortunately, not tolerated.

### Example
An example is not provided due to complexity of implementation. View the [NetworkPacketAnalyzer](https://github.com/robertdurfee/networkpacketanalyzer) for an implementation.
