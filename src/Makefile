#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------

# Platform Overrides
PLATFORM=HOST

#Deliverable name
PROJECT_NAME=c1m2

# Architectures Specific Flags
LINKER_FILE=../msp432p401r.lds
CPU=cortex-m4
ARCH=armv7e-m
SPECS=nosys.specs

# Compiler Flags and Defines
CC= #Compiler
LD= #Linker
LDFLAGS= # C program linker flags
CFLAGS= -Wall -Werror -g -std=c99 #C program flags
CPPFLAGS = #C preprocessor flags
DUMP= #objdump command

#Removal flags
REMOVE_FLAGS = main.i main.asm main.o main.d \
memory.i memory.asm memory.o memory.d \
interrupts_msp432p401r_gcc.i interrupts_msp432p401r_gcc.asm interrupts_msp432p401r_gcc.o interrupts_msp432p401r_gcc.d \
startup_msp432p401r_gcc.i startup_msp432p401r_gcc.asm startup_msp432p401r_gcc.o startup_msp432p401r_gcc.d \
system_msp432p401r.i system_msp432p401r.asm system_msp432p401r.o system_msp432p401r.d \
c1m2.out c1m2.map a.out

ifeq ($(PLATFORM),HOST)
CC=gcc
LD=ld
CPPFLAGS=-DHOST
LDFLAGS= -Wl,-O0 -Wl,-Map=$(PROJECT_NAME).map
DUMP=objdump
# etc
else
CC=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-gcc
LD=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-ld
CPPFLAGS=-DMSP432 -mcpu=$(CPU) -march=$(ARCH) -specs=$(SPECS) -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16
LDFLAGS= -Wl,-O0  -Wl,-T $(LINKER_FILE) -Wl,-Map=$(PROJECT_NAME).map
DUMP=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin./arm-none-eabi-objdump

	# etc
endif

include	sources.mk


OBJS = $(SOURCES:.c=.o)
DEPS = $(SOURCES:.c=.d)

#Generate preprocessed file .i
%.i : %.c
	$(CC) -E $^ $(CFLAGS) $(CPPFLAGS) $(INCLUDES) -o $@

#Generate assembled file .asm
%.asm : %.c
	$(CC) -c $^ $(CFLAGS) $(CPPFLAGS) $(INCLUDES) -o $@
	$(DUMP) -S $@

#Generate object file .o
%.o : %.c
	$(CC) -c $^ $(CFLAGS) $(CPPFLAGS) $(INCLUDES) -o $@

#Cleans all built files
.PHONY: clean
clean:
	rm $(REMOVE_FLAGS)

#Generate object files of all source files
.PHONY: compile-all
compile-all: $(SOURCES)
	$(CC) -c $^ $(CFLAGS) $(CPPFLAGS) $(INCLUDES) 

.PHONY: build
build: $(PROJECT_NAME).out
$(PROJECT_NAME).out: $(OBJS)
	#Generate .d files
	$(CC) $(LDFLAGS) $(CFLAGS) $(CPPFLAGS) $(INCLUDES) -MD $(SOURCES)
	#Compile into c1m2.out
	$(CC) $(LDFLAGS) $(CFLAGS) $(CPPFLAGS) $(INCLUDES) -o $@ $^



