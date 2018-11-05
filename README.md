# CmdParser

An easy to use Command Line Parser and Common Workflow Language (CWL) specification creater/reader

Credits to YAML-CPP (https://github.com/jbeder/yaml-cpp/)

## Usage

After adding the files to the compilation, the CmdParser can be used in the following way:

```cpp/
#include "cbicaCmdParser.h"

int main(int argc, char** argv)
{
  auto parser = cbica::CmdParser(argc, argv);
  parser.addRequiredParameter("i", "images", cbica::Parameter::FILE, "NIfTI or DICOM", "Input coregistered image(s) to load into application", "Multiple images are delineated using ','");
  parser.addOptionalParameter("m", "mask", cbica::Parameter::FILE, "NIfTI or DICOM", "Input mask [coregistered with image(s)] to load into application", "Accepts only one file");
  parser.exampleUsage("-i C:/data/input1.nii.gz,C:/data/input2.nii.gz -m C:/data/inputMask.nii.gz");
  parser.writeCWLFile("C:/algoDef/"); // always written as #exeName.cwl
  
  // do fancy processing
  
  return EXIT_SUCCESS;
}
```