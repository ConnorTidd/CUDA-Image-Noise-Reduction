# environment
SM := 35

GCC := g++
NVCC := nvcc

# Remove function
RM = rm -f
 
# Specify opencv Installation
#opencvLocation = /usr/local/opencv
opencvLIB= `pkg-config --libs opencv`
opencvINC= `pkg-config --cflags opencv`

# Compiler flags:
# -g    debugging information
# -Wall turns on most compiler warnings
GENCODE_FLAGS := -gencode arch=compute_$(SM),code=sm_$(SM)
LIB_FLAGS := -lcudadevrt -lcudart

NVCCFLAGS := -O3
GccFLAGS = -fopenmp -O3 

debug: GccFLAGS += -DDEBUG -g -Wall
debug: NVCCFLAGS = -g -G
debug: all

# The build target executable:
TARGET = denoise

all: build

build: $(TARGET)

$(TARGET): src/dlink.o src/main.o src/$(TARGET).o
	$(NVCC) $(NVCCFLAGS) $(opencvINC) $^ -o $@ $(GENCODE_FLAGS) $(opencvLIB) -link 

src/dlink.o: src/$(TARGET).o 
	$(NVCC) $(NVCCFLAGS) $^ -o $@ $(GENCODE_FLAGS) -dlink

src/main.o: src/main.cpp
	$(GCC) $(GccFLAGS) $(opencvLIB) $(opencvINC) -c $< -o $@
	
src/$(TARGET).o: src/$(TARGET).cu
	$(NVCC) $(NVCCFLAGS) -dc $< -o $@ $(GENCODE_FLAGS) 
	
clean:
	$(RM) $(TARGET) src/*.o *.o *.tar* *.core* *out*.jpg *input*.jpg
