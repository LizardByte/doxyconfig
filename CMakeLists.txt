# find doxygen and graphviz
find_package(Doxygen 1.10 REQUIRED dot)  # debian and ubuntu left in the dust

# get parent project docs directory
set(SOURCE_DOCS_DIR "${CMAKE_SOURCE_DIR}/docs")

# fail if the directory does not exist
if(NOT EXISTS ${SOURCE_DOCS_DIR})
    message(FATAL_ERROR "Directory ${SOURCE_DOCS_DIR} does not exist")
endif()

# define variables based on whether we are building on readthedocs
if(DEFINED ENV{READTHEDOCS})
    set(DOXYGEN_BUILD_DIR_CMAKE $ENV{READTHEDOCS_OUTPUT})
    set(DOXYGEN_PROJECT_VERSION $ENV{READTHEDOCS_VERSION})
else()
    set(DOXYGEN_BUILD_DIR_CMAKE "${CMAKE_CURRENT_BINARY_DIR}/build")
    set(DOXYGEN_PROJECT_VERSION ${PROJECT_VERSION})
endif()
message(STATUS "DOXYGEN_BUILD_DIR_CMAKE: ${DOXYGEN_BUILD_DIR_CMAKE}")

# create build directories, as doxygen fails to create it in some cases?
file(MAKE_DIRECTORY "${DOXYGEN_BUILD_DIR_CMAKE}/html")

# copy files to build directory
file(COPY_FILE "${CMAKE_CURRENT_SOURCE_DIR}/header.html" "${SOURCE_DOCS_DIR}/header-doxyconfig.html")
file(COPY_FILE "${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile" "${SOURCE_DOCS_DIR}/Doxyfile-doxyconfig")

# append the "${SOURCE_DOCS_DIR}/Doxyfile to the Doxyfile-doxyconfig
file(READ "${SOURCE_DOCS_DIR}/Doxyfile" DOXYFILE_CONTENTS)
file(APPEND "${SOURCE_DOCS_DIR}/Doxyfile-doxyconfig" "${DOXYFILE_CONTENTS}")

# sunshine has its own icon and logo
if(NOT ${CMAKE_PROJECT_NAME} STREQUAL "Sunshine")
    # download icon and logo
    file(DOWNLOAD
            "https://raw.githubusercontent.com/LizardByte/.github/master/branding/logos/favicon.ico"
            "${DOXYGEN_BUILD_DIR_CMAKE}/lizardbyte.ico"
    )
    file(DOWNLOAD
            "https://raw.githubusercontent.com/LizardByte/.github/master/branding/logos/logo-128x128.png"
            "${DOXYGEN_BUILD_DIR_CMAKE}/lizardbyte.png"
    )
endif()

# set FONT_AWESOME_FILES depends
set(FONT_AWESOME_FILES_DEPENDS
        "${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/css/all.min.css"
        "${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/js/all.min.js"
        "${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/webfonts/"
)

find_program(NPM npm REQUIRED)
add_custom_target(_docs_fontawesome_install
        COMMENT "Installing node modules"
        BYPRODUCTS ${FONT_AWESOME_FILES_DEPENDS}
        COMMAND ${NPM} install
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
        VERBATIM
)

# copy Font Awesome files
add_custom_command(
        OUTPUT  FONT_AWESOME_FILES
        COMMAND ${CMAKE_COMMAND}
                -E copy ${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/css/all.min.css
                    ${DOXYGEN_BUILD_DIR_CMAKE}/html/assets/fontawesome/css/all.min.css
        COMMAND ${CMAKE_COMMAND}
                -E copy ${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/js/all.min.js
                    ${DOXYGEN_BUILD_DIR_CMAKE}/html/assets/fontawesome/js/all.min.js
        COMMAND ${CMAKE_COMMAND}
                -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR}/node_modules/@fortawesome/fontawesome-free/webfonts
                    ${DOXYGEN_BUILD_DIR_CMAKE}/html/assets/fontawesome/webfonts
        COMMENT "Copying Font Awesome files"
        DEPENDS ${FONT_AWESOME_FILES_DEPENDS}
)

# convert to relative path, so doxygen doesn't get confused on Windows
file(RELATIVE_PATH DOXYGEN_BUILD_DIR_RELATIVE "${SOURCE_DOCS_DIR}" "${DOXYGEN_BUILD_DIR_CMAKE}")
message(STATUS "DOXYGEN_BUILD_DIR_RELATIVE: ${DOXYGEN_BUILD_DIR_RELATIVE}")

# build docs
add_custom_target(docs ALL
        COMMENT "Building Doxygen documentation"
        WORKING_DIRECTORY "${SOURCE_DOCS_DIR}"
        COMMAND ${CMAKE_COMMAND} -E env
            READTHEDOCS_OUTPUT=${DOXYGEN_BUILD_DIR_RELATIVE}
            READTHEDOCS_VERSION=${DOXYGEN_PROJECT_VERSION}
            ${DOXYGEN_EXECUTABLE} Doxyfile-doxyconfig
        VERBATIM
        DEPENDS FONT_AWESOME_FILES
)
