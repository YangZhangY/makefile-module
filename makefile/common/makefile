.PHONY : all
#DIR_BUILD := /home/jesson/study/makefile/build
#DIR_COMMON_INC := /home/jesson/study/makefile/common/inc

DIR_SRC := src
DIR_INC := inc

TYPE_INC := .h
TYPE_SRC_C := .c
TYPE_SRC_CPP := .cpp

TYPE_OBJ := .o
TYPE_DEP := .dep

AR := ar
ARFLAGS := crs

CC  := gcc
CXX := g++
CFLAGS   := -I$(DIR_INC) -I$(DIR_COMMON_INC)
CXXFLAGS := -I$(DIR_INC) -I$(DIR_COMMON_INC)

ifeq ($(DEBUG), true)
CFLAGS += -g
endif 

MODULE := $(realpath .)
MODULE := $(notdir $(MODULE))
DIR_OUTPUT := $(addprefix $(DIR_BUILD)/, $(MODULE))

OUTPUT := $(MODULE).a
## 增加前缀
OUTPUT := $(addprefix $(DIR_BUILD)/, $(OUTPUT))


## 先得到所有的源文件
SRCS_C := $(wildcard $(DIR_SRC)/*$(TYPE_SRC_C))
## 更换后缀 src/func.o
OBJS_C := $(SRCS_C:$(TYPE_SRC_C)=$(TYPE_OBJ))

SRCS_CPP := $(wildcard $(DIR_SRC)/*$(TYPE_SRC_CPP))
OBJS_CPP := $(SRCS_CPP:$(TYPE_SRC_CPP)=$(TYPE_OBJ))

pre_objs_c := $(OBJS_C)
## 更换前缀
## 通配符替换, 相当于换个前缀
OBJS_C := $(patsubst $(DIR_SRC)/%,$(DIR_OUTPUT)/%, $(OBJS_C))
OBJS_CPP := $(patsubst $(DIR_SRC)/%,$(DIR_OUTPUT)/%, $(OBJS_CPP))

DEPS := $(SRCS:$(TYPE_SRC)=$(TYPE_DEP))
DEPS := $(patsubst $(DIR_SRC)/%, $(DIR_OUTPUT)/%, $(DEPS))

vpath %$(TYPE_INC) $(DIR_INC)
vpath %$(TYPE_INC) $(DIR_COMMON_INC)
vpath %$(TYPE_SRC) $(DIR_SRC)

-include $(DEPS)

#all: 
all: $(OUTPUT)
	@echo "$(DIR_OUTPUT)"
	@echo "$(pre_objs)"
	@echo "$(OBJS_C)"
	@echo "$(OUTPUT)"
	@echo "$(DEPS)"
	@echo "$(TYPE_SRC)"

$(OUTPUT) : $(OBJS_C) $(OBJS_CPP)
	echo "ar to $@"
	$(AR) $(ARFLAGS)  $@ $^
## 编译目录下的 .c filter 过滤
$(DIR_OUTPUT)/%$(TYPE_OBJ) : %$(TYPE_SRC_C)
	@echo "objssss $@"
	@echo "create objs $^"
	$(CC) $(CFLAGS) -o $@ -c $(filter %$(TYPE_SRC_C), $^)

$(DIR_OUTPUT)/%$(TYPE_OBJ) : %$(TYPE_SRC_CPP)
	@echo "objssss $@"
	@echo "create objs $^"
	$(CXX) $(CXXFLAGS) -o $@ -c $(filter %$(TYPE_SRC_CPP), $^)

$(DIR_OUTPUT)/%$(TYPE_DEP):%$(TYPE_SRC_C)
	@echo "Creating deps $@"
	$(CC) $(CFLAGS) -MM -E $(filter %$(TYPE_SRC_C), $^) | sed 's,\(.*\)\.o[ :]*,$(DIR_OUTPUT)/\1.o $@ : ,g' > $@

$(DIR_OUTPUT)/%$(TYPE_DEP):%$(TYPE_SRC_CPP)
	@echo "Creating deps $@"
	$(CXX) $(CXXFLAGS) -MM -E $(filter %$(TYPE_SRC_CPP), $^) | sed 's,\(.*\)\.o[ :]*,$(DIR_OUTPUT)/\1.o $@ : ,g' > $@



