binwalk -OPTIONS  -extractionOptions  FILE1 FILE2 FILE3

# Disassembly Scan Options: 
	-Y, --disasm Identify the CPU architecture of a file using the capstone disassembler 
	-T, --minsn=<int> Minimum number of consecutive instructions to be considered valid (default: 500) 
	-k, --continue Don't stop at the first match

# Signature Scan Options:
	-B, --signature Scan target file(s) for common file signatures 
	-R, --raw=<str> Scan target file(s) for the specified sequence of bytes 
	-A, --opcodes Scan target file(s) for common executable opcode signatures 
	-m, --magic=<file> Specify a custom magic file to use 
	-b, --dumb Disable smart signature keywords 
	-I, --invalid Show results marked as invalid 
	-x, --exclude=<str> Exclude results that match <str>
	-y, --include=<str> Only show results that match <str>

# Extraction Options:
	-e, --extract Automatically extract known file types 
	-D, --dd=<type[:ext[:cmd]]> Extract <type> signatures (regular expression), give the files an extension of <ext>, and execute <cmd> 
	-M, --matryoshka Recursively scan extracted files 
	-d, --depth=<int> Limit matryoshka recursion depth (default: 8 levels deep) 
	-C, --directory=<str> Extract files/folders to a custom directory (default: current working directory) 
	-j, --size=<int> Limit the size of each extracted file 
	-n, --count=<int> Limit the number of extracted files 
	-0, --run-as=<str> Execute external extraction utilities with the specified user's privileges 
	-1, --preserve-symlinks Do not sanitize extracted symlinks that point outside the extraction directory (dangerous) 
	-r, --rm Delete carved files after extraction 
	-z, --carve Carve data from files, but don't execute extraction utilities 
	-V, --subdirs Extract into sub-directories named by the offset