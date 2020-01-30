# CmdParser

An easy to use Command Line Parser and Common Workflow Language (CWL) specification creater/reader.

Credits to YAML-CPP (https://github.com/jbeder/yaml-cpp/).

Main development happens in https://github.com/CBICA/CaPTk from where we regularly do merges.

## Usage

Add this repo as a submodule and invoke it from the main <b>CMakeLists.txt</b> file:

```bash
ADD_SUBDIRECTORY(CmdParser) # that's all you would need to do to build the CmdParser library

TARGET_LINK_LIBRARIES( 
  ${EXECUTABLE_YOU_WANT_TO_INFUSE_WITH_FUNCTIONALITY}
  CmdParser
)
```

After adding the files to the compilation, the CmdParser can be used in the following way in the source code:

```cpp
#include "cbicaCmdParser.h"

int main(int argc, char** argv)
{
  auto parser = cbica::CmdParser(argc, argv);
  parser.addRequiredParameter("i", "images", cbica::Parameter::FILE, "NIfTI or DICOM", "Input coregistered image to load into application");
  parser.addOptionalParameter("m", "mask", cbica::Parameter::FILE, "NIfTI or DICOM", "Input mask [coregistered with image] to load into application");
  parser.exampleUsage("-i C:/data/input1.nii.gz,C:/data/input2.nii.gz -m C:/data/inputMask.nii.gz"); // deprecated; see compilation warning for details
  parser.writeCWLFile("C:/algoDef/"); // always written as ${exeName}.cwl
  
  std::string file_inputImage, file_inputMask;
  
  parser.getParameterValue("i", file_inputImage);

  if (parser.isPresent("m"))
  {
    parser.getParameterValue("m", file_inputMask);
  }
  else
  {
    // initialize empty mask or however the default case is handled
  }
  
  // getParameterValue supports std::string, float, int, size_t and bool types
  
  // do fancy processing
  
  return EXIT_SUCCESS;
}
```