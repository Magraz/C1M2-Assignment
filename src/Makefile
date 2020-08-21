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
DUMP= #objdump toolchain command
SIZE = #size toolchain command

ifeq ($(PLATFORM),HOST)
CC=gcc
LD=ld
SIZE = size
DUMP=objdump
CPPFLAGS=-DHOST
LDFLAGS= -Wl,-O0 -Wl,-Map=$(PROJECT_NAME).map

else
CC=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-gcc
LD=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-ld
DUMP=/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-objdump
SIZE =/home/magraz/Downloads/gcc-arm-none-eabi-9-2020-q2-update/bin/arm-none-eabi-size
CPPFLAGS=-DMSP432 -mcpu=$(CPU) -march=$(ARCH) -specs=$(SPECS) -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16
LDFLAGS= -Wl,-O0  -Wl,-T $(LINKER_FILE) -Wl,-Map=$(PROJECT_NAME).map

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
	rm -f *.o *.i *.asm *.d *.out $(PROJECT_NAME).map

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
	#Size
	$(SIZE) $(PROJECT_NAME).out



