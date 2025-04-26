# Fastdep - C/C++ dependancy caching script

<p align="center">
  
![image](https://github.com/user-attachments/assets/362d5f35-b47e-46ad-b952-de93cbd24d6c)
  
</p>


## Usage
`fastdep [-d <depfile>] [-o <object>] [--compiler <compiler>] <file>` \
returns: `<file> <dep1> <dep2> <dep3> ...` in stdout

`<compiler>` must support the `-MM` flag such as gcc \
`<object>` is the object `<file>` compiles to

the following is an example of usage within a shell script, fastdep is used to skip files we know we dont need to compile
```sh
objects=""
for file in src/*.c
do
	out=${file/src/$BUILD_DIR}.o
	objects="$objects $out"
	
	skip=true
	
	for dep in $(fastdep $file -o $out -d $DEPFILE)
	do
		if [ $file -nt $out ];
		then
			skip=false;
		fi
	done

	if $skip;
	then
		echo × skipping $file → $out
		continue
	fi
	
	echo $CC -c $CCFLAGS $file -o $out
	$CC -c $CCFLAGS $file -o $out
done

$CC $objects -o $TARGET
```

