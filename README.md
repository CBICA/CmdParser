# CmdParser

An easy to use Command Line Parser and Common Workflow Language (CWL) specification creater/reader

Credits to YAML-CPP (https://github.com/jbeder/yaml-cpp/)

## Usage

In the main <b>CMakeLists.txt</b> file, add the following lines:

```cpp
# for YAML-CPP
INCLUDE_DIRECTORIES( ${PROJECT_SOURCE_DIR}/thirdparty/yaml-cpp/include )
FILE(GLOB_RECURSE YAMLCPP_Headers "${PROJECT_SOURCE_DIR}/thirdparty/yaml-cpp/include/*.h")
FILE(GLOB_RECURSE YAMLCPP_Sources "${PROJECT_SOURCE_DIR}/thirdparty/yaml-cpp/src/*.cpp")
SET( CMDPARSER_HEADERS ${YAMLCPP_Headers} CACHE STRING "YAML-CPP headers" FORCE )
SET( CMDPARSER_SOURCES ${YAMLCPP_Sources} CACHE STRING "YAML-CPP sources" FORCE )

# for CmdParser
INCLUDE_DIRECTORIES( ${PROJECT_SOURCE_DIR}/include )
SET( CMDPARSER_HEADERS ${CMDPARSER_HEADERS} "${PROJECT_SOURCE_DIR}/include/cbicaCmdParser.h" )
SET( CMDPARSER_SOURCES ${CMDPARSER_HEADERS} "${PROJECT_SOURCE_DIR}/include/cbicaCmdParser.cpp" )

## add ${CMDPARSER_HEADERS} and ${CMDPARSER_SOURCES} to project compilation
```
After adding the files to the compilation, the CmdParser can be used in the following way in the source code:

```cpp
#include "cbicaCmdParser.h"

int main(int argc, char** argv)
{
  auto parser = cbica::CmdParser(argc, argv);
  parser.addRequiredParameter("i", "images", cbica::Parameter::FILE, "NIfTI or DICOM", "Input coregistered image(s) to load into application", "Multiple images are delineated using ','");
  parser.addOptionalParameter("m", "mask", cbica::Parameter::FILE, "NIfTI or DICOM", "Input mask [coregistered with image(s)] to load into application", "Accepts only one file");
  parser.exampleUsage("-i C:/data/input1.nii.gz,C:/data/input2.nii.gz -m C:/data/inputMask.nii.gz");
  parser.writeCWLFile("C:/algoDef/"); // always written as ${exeName}.cwl
  
  // do fancy processing
  
  return EXIT_SUCCESS;
}
```