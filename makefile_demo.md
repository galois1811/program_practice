[GNU make](https://www.gnu.org/software/make/manual/make.html)
 

## 以如下的目录结构为例的makefile demo
<pre>
├─.vscode
├─cppunit
│  ├─include
│  │  └─cppunit
│  │      ├─config
│  │      ├─extensions
│  │      ├─plugin
│  │      ├─portability
│  │      ├─tools
│  │      └─ui
│  │        ├─mfc
│  │        ├─qt
│  │        └─text
│  └─lib
├─project       (makefie所在目录)
├─source        (源码所在目录)
└─testcase      (测试用例所在目录)
</pre>


 ```make
 ROOT_DIR = ..

INCLUDES = -I$(ROOT_DIR)/cppunit/include
LIBS = $(ROOT_DIR)/cppunit/lib/libcppunit.a

subdirs = $(shell find $(ROOT_DIR)/ -type d)
source_files = $(foreach dir, $(subdirs), $(wildcard $(dir)/*.cpp))
head_files = $(foreach dir, $(subdirs), $(wildcard $(dir)/*.h))

objs = $(patsubst %.cpp, %.o, $(notdir $(source_files)))

source_dirs = $(dir $(source_files))

#定义一个空格
#space := 
#space +=
#space := $(subst ,, )
space := $(eval) $(eval)
#VPATH是通过":"来区隔多个路径的，把源文件路径中的空格替换为":"
VPATH = $(subst $(space),:,$(source_dirs))

head_dirs = $(dir $(head_files))
head_path = $(subst $(space), -I,$(head_dirs))
INCLUDES += -I$(head_path)

#调试用：find的结果可能包含换行或者多个空格，在把它作为subst的字符串入参时，先通过echo打印一下，可实现格式化。
top_dirs = ../project ../source ../testcase
all_subdirs = $(foreach dir, $(top_dirs), $(shell find $(dir) -type d))
test_vpath1 = $(subst $(space),:,$(all_subdirs))
test_vpath2 = $(subst $(space),:,$(shell echo $(all_subdirs)))


%.o:%.cpp
	@echo building $@ ...
	@g++ -g -c $(INCLUDES) $< -o ./$(notdir $@)

all:$(objs)
	g++ -g -o target $(objs) $(LIBS)

#该伪目标为makefile调试用
.PHONY:debug
debug:
	@echo $(ROOT_DIR)
	@echo $(INCLUDES)
	@echo $(LIBS)
	@echo $(subdirs)
	@echo $(source_files)
	@echo $(objs)
	@echo $(source_dirs)
	@echo $(VPATH)
	@echo $(all_subdirs)
	@echo $(test_vpath1)
	@echo $(test_vpath2)
	@echo "done"


.PHONY:clean
clean:
	-rm -f ./*.o
	-rm -f ./target
	-rm -f ./*.xml
```

