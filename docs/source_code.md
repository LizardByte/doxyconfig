# Source Code

Code should be documented using Doxygen syntax.

## Guidelines

### Doxygen Comments

* Use Doxygen comments to document all files, functions, classes, and variables.
* [Inline documenation block](#inline-documentation-blocks) should use the following format:

  @verbatim
///< A brief description of the member.
@endverbatim

* Multi-line comments, such as for a [documentation block](#documentation-blocks), should use the following format:

  @verbatim
/**
* @brief A brief description of the member.
* More detailed description of the member.
*/
@endverbatim

### Documentation Blocks

Documentation blocks should be placed above the declaration of the function, class, or variable. Below is an example
of a documentation block for the main function.

@verbatim
/**
 * @brief Main application entry point.
 * @param argc The number of arguments.
 * @param argv The arguments.
 * @return The exit code.
 * @examples
 * main(1, const char* args[] = {"hello", "markdown", nullptr});
 * @examples_end
 */
int main(int argc, char *argv[]);
@endverbatim

@note The \@examples and \@examples_end tags are not standard Doxygen tags. They are custom aliases
we have specified to simplify documenting examples. Do not confuse this with the standard \@example tag.

In some cases, it could be valuable to have slightly different documentation for the definitions, especially when
the definition may change depending on the platform or other factors. In such cases, you should put the documentation
that is common in the declaration and the definition-specific documentation in the definition. Below is an example of
how to document the declaration and definition of a function.

@verbatim
// myFile.h
/**
 * @brief A brief description of the function.
 * @param arg1 Describe the first argument.
 * @param arg2 Describe the second argument.
 * @return Describe the result.
 */
int myFunction(int arg1, int arg2);

// myFile.cpp
/**
 * This describes anything which is specific to the implementation of the function.
 */
int myFunction(int arg1, int arg2)
{
    // Implementation
}
@endverbatim

### File Documentation

The file documentation block must be placed at the top of the file. If it is not present, Doxygen will ignore the file.
Understandably, it is difficult to make a creative description for every file, although it is still required.
Below is an example of a file documentation block.

@verbatim
/**
 * @file src/main.cpp
 * @brief Main application entry point.
 */
@endverbatim

### Inline Documentation Blocks

Inline comments can be used to describe enum values, variables, and other code constructs.
To document the members of a file, struct, union, class, or enum, it is sometimes desired to place the documentation
block after the member instead of before. For this purpose you have to put an additional `<` marker in the comment
block. Below is an example of an inline comment for an enum value.

@verbatim
enum class MyEnum
{
   FIRST_VALUE,  ///< A brief description of the FIRST_VALUE
   SECOND_VALUE  ///< A brief description of the SECOND_VALUE
};
@endverbatim

### Custom Aliases

We have defined some custom aliases to simplify documenting examples.

* \@examples - Start of an example block. This will format the following text as `cpp`.
* \@examples_end - End of an example block.
* \@fa_icon{font awsome definition} - Insert a Font Awesome icon. Give the icon style and name as arguments.
  e.g. \@fa_icon{fa-solid fa-check}.
* Admonitions
  * @admonition{The one with the custom titles | \@admonition{Custom Title | Content } -
    It’s got a certain charm to it.}
  * @attention{\@attention{Content} - The sun causes global warming.}
  * @caution{\@caution{Content} - Cliff ahead: Don’t drive off it.}
  * @danger{\@danger{Content} - Mad scientist at work!}
  * @error{\@error{Content} - Does not compute.}
  * @hint{\@hint{Content} - Insulators insulate, until they are subject to ______ voltage.}
  * @important{\@important{Content} - Tech is not neutral, nor is it apolitical.}
  * @note{\@note{Content} - This is a note.}
  * @seealso{\@seealso{Content} - Other relevant information.}
  * @tip{\@tip{Content} - 25% if the service is good.}
  * @todo{\@todo{Content} - Fix this.}
  * @warning{\@warning{Content} - Reader discretion is strongly advised.}
