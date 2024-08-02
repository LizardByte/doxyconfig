# doxyconfig

This is a common Doxygen config for LizardByte projects.

## Usage

1. Add this repository as a submodule to your project.

   ```bash
   git submodule add https://github.com/LizardByte/doxyconfig.git third-party/doxyconfig
   ```

2. Place project specific Doxyfile config in `./docs/Doxyfile`. You can overwrite anything from the common config here.
3. Add the following to your CMakeLists.txt file.

   ```cmake
   option(BUILD_DOCS "Build documentation" ON)
   if(BUILD_DOCS)
       add_subdirectory(third-party/doxyconfig docs)
   endif()
   ```
