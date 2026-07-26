<div align="center">
  <img
    src="https://raw.githubusercontent.com/LizardByte/.github/refs/heads/master/branding/logos/logo.svg"
    alt="LizardByte icon"
    width="256"
  />
  <h1 align="center">doxyconfig</h1>
  <h4 align="center">Common doxygen config for LizardByte projects.</h4>
</div>

<div align="center">
  <a href="https://sonarcloud.io/project/overview?id=LizardByte_doyxconfig"><img src="https://img.shields.io/sonar/quality_gate/LizardByte_doxyconfig.svg?server=https%3A%2F%2Fsonarcloud.io&style=for-the-badge&logo=sonarqubecloud&label=sonarcloud" alt="SonarCloud"></a>
</div>

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

4. Add the following to your `.gitignore` file.

   ```gitignore
   # doxyconfig
   docs/doxyconfig*
   ```

5. Optionally, add the following to the input list in your Doxyfile.

   ```doxyfile
   INPUT += ../third-party/doxyconfig/docs/source_code.md
   ```

6. Optionally, copy the `.readthedocs.yaml` file to the root of your project.

<details style="display: none;">
  <summary></summary>
  [TOC]
</details>
