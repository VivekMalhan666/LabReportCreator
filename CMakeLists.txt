cmake_minimum_required(VERSION 3.0)

# Main Step 0: Add CMake dependencies
# ------------------------------------------------------------------------------------------------------
MESSAGE(STATUS "Adding CMake modules...")
LIST(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules/cmake-pandocology")
INCLUDE(pandocology)
INCLUDE(cmspandoc)

# Main Step I: Create main project
# -------------------------------------------------------------------------------
file(READ src/description.md desc)
file(READ src/code.c code)
file(READ src/input.txt input)
file(READ src/output.txt output)
#execute_process(
#	COMMAND mmdc -i src/img.mmd -o src/img/img.png
#	)
file(REMOVE src/finaloutput.md)
file(APPEND src/finaloutput.md    ${desc}
	"\n# FLOW CHART\n"
	"![FLOW CHART\n](img/img.png)\n "
	"\n \n"
	"\n# CODE\n"
	"```cpp\n"
	"${code}\n"
	"```"
	"\n# INPUT\n"
	${input}
	"\n# OUTPUT\n"
	${output}
	)
add_document(
    TARGET		finaloutput
    OUTPUT_FILE 	finaloutput.pdf
    SOURCES		src/finaloutput.md
    #RESOURCE_FILES	./src/img.png
    RESOURCE_DIRS	src/img
    PANDOC_DIRECTIVES 	${PANDOC_PDF_CONFIG}
)
