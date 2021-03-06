CDIR = ../commons

DEBUG = -g 
#-DNDEBUG
#-g -fverbose-asm -masm=intel -Wa,-alhn=$@
#
INTEL_CC = icc
INTEL_CFLAGS = $(DEBUG) -I"$(CDIR)" -O3 -march=core-avx2 -std=c++14 -inline-factor=500 -no-inline-max-size -no-inline-max-total-size -use-intel-optimized-headers -parallel -qopt-prefetch=4 -qopt-mem-layout-trans=2 -pthread -c
INTEL_LFLAGS = $(DEBUG) -I"$(CDIR)" -O3 -march=core-avx2 -std=c++14 -inline-factor=500 -no-inline-max-size -no-inline-max-total-size -use-intel-optimized-headers -parallel -qopt-prefetch=4 -qopt-mem-layout-trans=2 -pthread
INTEL_TFLAGS = -I"$(CDIR)" -fast -DNDEBUG -std=c++11 -inline-factor=500 -no-inline-max-size -no-inline-max-total-size -use-intel-optimized-headers -parallel -qopt-prefetch=4 -qopt-mem-layout-trans=3 -pthread 

GNU_CC = g++
GNU_CFLAGS = $(DEBUG) -I"$(CDIR)" -O3 -march=core-avx2 -ffloat-store -pthread -std=c++14 -c
GNU_LFLAGS = $(DEBUG) -I"$(CDIR)" -O3 -march=core-avx2 -ffloat-store -pthread -std=c++14

CC = $(GNU_CC)
CFLAGS = $(GNU_CFLAGS)
LFLAGS = $(GNU_LFLAGS)

SOURCEDIR = .
BUILDDIR = ../../build
EXECUTABLE = ../../dist/parallelLDA

SOURCES = $(wildcard $(SOURCEDIR)/*.cpp)
OBJECTS = $(patsubst $(SOURCEDIR)/%.cpp,$(BUILDDIR)/%.o,$(SOURCES))

COMMONS = $(wildcard $(SOURCEDIR)/../commons/*.cpp)
COBJECT = $(patsubst $(SOURCEDIR)/../commons/%.cpp,$(BUILDDIR)/%.o,$(COMMONS))

all: $(EXECUTABLE)

gcc: $(EXECUTABLE)

intel: CC=$(INTEL_CC)
intel: CFLAGS=$(INTEL_CFLAGS)
intel: LFLAGS=$(INTEL_LFLAGS)
intel: $(EXECUTABLE)
 
$(EXECUTABLE): $(COBJECT)  $(OBJECTS)
	$(CC) $(LFLAGS) $^ -o $@

$(OBJECTS): $(BUILDDIR)/%.o : $(SOURCEDIR)/%.cpp
	$(CC) $(CFLAGS) $< -o $@

$(COBJECT): $(BUILDDIR)/%.o : $(SOURCEDIR)/../commons/%.cpp
	$(CC) $(CFLAGS) $< -o $@

inteltogether:
	$(INTEL_CC) $(INTEL_TFLAGS) $(SOURCES) $(COMMONS) -o $(EXECUTABLE)

clean:
	rm -rf $(BUILDDIR)/*
	rm -rf $(EXECUTABLE)
