ifeq ($(OS),Windows_NT)
SRC_CPP := $(shell dir /S /B | findstr /i .cpp$)
SRC_CU := $(shell dir /S /B | findstr /i .cu$)
OBJ_CURR := $(shell dir /S /B | findstr /i .o$)
OUT := $(notdir $(shell cd)).exe
INC := -I"../SDL_Lib\include/"
LIBPATH := -L"C:\Git\NekoEngine\SDL_Lib\lib\x64"
else
#colors (wont work in windows)
ccred:="\033[46;31m"
ccyellow:="\033[46;34m"
ccgreen:="\033[40;32m"
ccend:="\033[0m"
SRC_CU := $(shell find -name '*.cu')
SRC_CPP := $(shell find -name '*.cpp')
#only used for cls function
OBJ_CURR := $(shell find -name '*.o')
INC := -I/usr/local/cuda-7.0/targets/x86_64-linux/include/ -I/usr/local/cuda-7.0/samples/common/inc/
OUT := $(notdir $(shell pwd))
LIBPATH = -L"/usr/local/cuda-7.0/lib64"
endif



OBJ_CU := $(SRC_CU:%.cu=%.o)
OBJ_CPP := $(SRC_CPP:%.cpp=%.o)
OBJ_ALL := $(OBJ_CU) $(OBJ_CPP)

ifeq "$(OBJ_CU)" ""
OBJ_CUDA_L :=
else
OBJ_CUDA_L := cudatemp.o
endif

NVCC := nvcc
GCC := g++
GCCFLAG :=  -O3 -fopenmp -w
NVARCH := -arch=sm_30
NVCCFLAG := -std=c++11 -O3

LIBLST = -lSDL2 -fopenmp

$(OUT): $(OBJ_CPP) NVCCLINK
	@echo $(FOLDER)
	@echo $(ccgreen)link all objects:$(ccend)
ifeq ($(OS),Windows_NT)
#echo stuff...
else
	@echo $(ccgreen)"$(OBJ_ALL)"$(ccend)| sed -e 's/ /\x1b[0m\n\x1b[40;32m/g'
endif
	@$(GCC) -o $(OUT) $(OBJ_CUDA_L) $(OBJ_ALL) $(LIBPATH) $(LIBLST)
ifeq ($(OS),Windows_NT)
#delete for loop
	@for %%x in ($(OBJ_ALL)) do echo delete: %%x
	@for %%x in ($(OBJ_ALL)) do del %%x
else
	@for ob in $(OBJ_ALL) ; do \
	echo $(ccred) delete: $$ob $(ccend);rm -rf $$ob ; \
	done
endif
ifeq "$(OBJ_CUDA_L)" ""
	@echo $(ccred) delete: $(OBJ_CUDA_L) $(ccend)
endif
	@rm -rf $(OBJ_CUDA_L)
	@echo $(ccgreen)Object $(OUT) successfully compiled$(ccend)
	@echo "Makefile by Neko <nekoyuke@gmail.com>"

NVCCLINK: $(OBJ_CU)
ifeq "$(OBJ_CU)" ""
	@echo $(ccgreen)No CUDA files to link$(ccend)
else
	@echo $(ccgreen)CUDA-link objects:$(ccend)
ifeq ($(OS),Windows_NT)
	@for %%x in ($(OBJ_ALL)) do echo %%x
else
	@echo $(ccgreen)"$(OBJ_CU)"$(ccend)| sed -e 's/ /\x1b[0m\n\x1b[40;32m/g'
endif
	@$(NVCC) $(NVCCFLAG) $(NVARCH) -dlink -o $(OBJ_CUDA_L) $(OBJ_CU) $(LIBPATH) $(LIBLST)
endif

%.o: %.cu
	@echo $(ccyellow)CUDA $(NVCC) compile:$<$(ccend)
	@$(NVCC) $(NVCCFLAG) $(NVARCH) $(INC) -c -o $@ $<

%.o: %.cpp
	@echo $(ccyellow)std $(GCC) compile:$<$(ccend)
	@$(GCC) $(GCCFLAG) $(INC) -std=c++11 -c -o $@ $<

.PHONY: clean
clean:
	@echo $(ccred) delete: $(OUT) $(ccend)
ifeq ($(OS),Windows_NT)
	@del $(OUT)
#delete for loop
	@for %%x in ($(OBJ_CURR)) do echo delete: %%x
	@for %%x in ($(OBJ_CURR)) do del %%x
else
	@rm -rf $(OUT)
	@for ob in $(OBJ_CURR) ; do \
	echo $(ccred) delete: $$ob $(ccend);rm -rf $$ob ; \
	done
endif