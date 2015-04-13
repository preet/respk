##respk
respk is a simple util to embed resources described using JSON within c++ source files.

###License
Apache License, version 2.0

###Dependencies
 * c++11
 * picojson (single-header BSD licensed json parser, included)

###Building
Building respk is trivial as its only a single source file and header.

###Usage
Resources must be described with a JSON file that looks like:


```json
{
    "file":"respk_some_images.cpp",
    "prepend":"namespace my { namespace images {",
    "append":"} }",
    "resources": [
        {
            "name":"cookie_jpg",
            "path":"some/path/cookie.png"
        },
        {
            "name":"cake_jpg",
            "path":"some/path/cake.png"
        }
    ]
}
```

* The prepend property is optional, it contains a string value for prepending the output file with code.
* The append property is optional, it contains a string value for appending the output file with code. The example shows how to wrap the resources in a namespace.
* The resources array must contain objects that have a 'name' and a 'path' property. The 'name' specifies the name of the data container for the resource in the source file. The 'path' is the file path for the resouce.
 * Note: The path may be a full path or relative to the JSON file

Once you've created your JSON resource description files, respk can be used to generate sources as follows:

```
./respk -o output/file.cpp -i path/to/json/desc/res1.json path/to/another/desc/res2.json
```
* Pass in the output file with the -o option
* Pass in one or more json files describing resources with the -i option

For the example JSON given above, respk might output something like:


```cpp

/* This file has been automatically generated by resgen.
 * Don't modify it unless you know what you're doing! */

#include <vector>

namespace my { namespace images {

// ============================================================= // 

extern std::vector<unsigned char> const cookie_jpg =
{
    0x31,0x32,0x33,0xa,0x61...
};

// ============================================================= // 

extern std::vector<unsigned char> const cake_jpg =
{
    0x63,0xa,0x78,0x79,0x7a...
};

} }
```
Any generated source files should be added to your project. You can then refer to the resources in your project using corresponding variable declarations:

```cpp

namespace my { namespace images {

extern std::vector<unsigned char> const cookie_jpg;
extern std::vector<unsigned char> const cake_jpg;

} }

int numbytes = my::images::cookie_jpg.size();
std::cout << "cookie.jpg is " << numbytes << " bytes" << std::endl;
```

