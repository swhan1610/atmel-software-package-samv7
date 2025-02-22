# ----------------------------------------------------------------------------
#         SAM Software Package License   
# ----------------------------------------------------------------------------
# Copyright (c) 2013, Atmel Corporation
#
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
# - Redistributions of source code must retain the above copyright notice,
# this list of conditions and the disclaimer below.
#
# Atmel's name may not be used to endorse or promote products derived from
# this software without specific prior written permission.
#
# DISCLAIMER: THIS SOFTWARE IS PROVIDED BY ATMEL "AS IS" AND ANY EXPRESS OR
# IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT ARE
# DISCLAIMED. IN NO EVENT SHALL ATMEL BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA,
# OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
# LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
# NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE,
# EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
# ----------------------------------------------------------------------------

#   Makefile for compiling the SMC NAND Flash Example project

#-------------------------------------------------------------------------------
#        User-modifiable options
#-------------------------------------------------------------------------------

# Chip & board used for compilation
# (can be overriden by adding CHIP=chip and BOARD=board to the command-line)
CHIP  = sama5d4x
BOARD = sama5d4x_ek

# Defines which are the available memory targets for the sama5d4x-EK board.
MEMORIES = ddram

# Trace level used for compilation
# (can be overriden by adding TRACE_LEVEL=#number to the command-line)
# TRACE_LEVEL_DEBUG      5
# TRACE_LEVEL_INFO       4
# TRACE_LEVEL_WARNING    3
# TRACE_LEVEL_ERROR      2
# TRACE_LEVEL_FATAL      1
# TRACE_LEVEL_NO_TRACE   0
TRACE_LEVEL = 4

# Optimization level, put in comment for debugging
OPTIMIZATION =

# Output file basename
OUTPUT = smc_nandflash

# Output directories
BIN = bin
OBJ = obj

#-------------------------------------------------------------------------------
#		Tools
#-------------------------------------------------------------------------------

# Tool suffix when cross-compiling
CROSS_COMPILE = arm-none-eabi-

# Libraries
LIBRARIES = ../../../../libraries
# Chip library directory
CHIP_LIB = $(LIBRARIES)/libchip_sama5d4x
# Board library directory
BOARD_LIB = $(LIBRARIES)/libboard_sama5d4x-ek
# Spiflash library directory
SPIFLASH_LIB = $(LIBRARIES)/libspiflash
# Spiflash library directory
SDMMC_LIB = $(LIBRARIES)/libsdmmc
# Nandflash library directory
NANDFLASH_LIB = $(LIBRARIES)/libnandflash
# Norflash library directory
NORFLASH_LIB = $(LIBRARIES)/libnorflash
# pmecc library directory
PMECC_LIB = $(LIBRARIES)/libpmecc

# USB library directory
USB_LIB = $(LIBRARIES)/usb

LIBS = -Wl,--start-group -lgcc -lc -lchip_$(CHIP)_gcc_rel -lboard_$(BOARD)_gcc_rel -lspiflash_gcc_rel -lsdmmc_gcc_rel -lnorflash_gcc_rel -lnandflash_gcc_rel -lpmecc_gcc_rel -Wl,--end-group

LIB_PATH = -L$(CHIP_LIB)/lib
LIB_PATH += -L$(BOARD_LIB)/lib
LIB_PATH += -L$(SPIFLASH_LIB)/lib
LIB_PATH += -L$(SDMMC_LIB)/lib
LIB_PATH += -L$(NANDFLASH_LIB)/lib
LIB_PATH += -L$(NORFLASH_LIB)/lib
LIB_PATH += -L$(PMECC_LIB)/lib
LIB_PATH+=-L=/lib
LIB_PATH+=-L=/../lib/gcc/arm-none-eabi/4.4.1

# Compilation tools
CC = $(CROSS_COMPILE)gcc
LD = $(CROSS_COMPILE)ld
SIZE = $(CROSS_COMPILE)size
STRIP = $(CROSS_COMPILE)strip
OBJCOPY = $(CROSS_COMPILE)objcopy
GDB = $(CROSS_COMPILE)gdb
NM = $(CROSS_COMPILE)nm

# Flags
INCLUDES  = -I$(CHIP_LIB)
INCLUDES += -I$(BOARD_LIB)
INCLUDES += -I$(SPIFLASH_LIB)
INCLUDES += -I$(SDMMC_LIB)
INCLUDES += -I$(NANDFLASH_LIB)
INCLUDES += -I$(NORFLASH_LIB)
INCLUDES += -I$(PMECC_LIB)
#INCLUDES += -I$(USB_LIB)/include
INCLUDES += -I$(LIBRARIES)

CFLAGS += -Wall -Wchar-subscripts -Wcomment -Wformat=2 -Wimplicit-int
CFLAGS += -Werror-implicit-function-declaration -Wmain -Wparentheses
CFLAGS += -Wsequence-point -Wreturn-type -Wswitch -Wtrigraphs 
CFLAGS += -Wuninitialized -Wunknown-pragmas -Wfloat-equal -Wundef
CFLAGS += -Wshadow -Wpointer-arith -Wbad-function-cast -Wwrite-strings
CFLAGS += -Waggregate-return -Wstrict-prototypes
CFLAGS += -Wmissing-prototypes -Wmissing-declarations
CFLAGS += -Wformat -Wmissing-format-attribute -Wno-deprecated-declarations
CFLAGS += -Wredundant-decls -Wnested-externs -Winline -Wlong-long
CFLAGS += -Wunreachable-code

# To reduce application size use only integer printf function.
CFLAGS += -Dprintf=iprintf

# -mlong-calls  -Wall
CFLAGS += -mcpu=cortex-a5 -ffunction-sections
CFLAGS += -g $(OPTIMIZATION) $(INCLUDES) -D$(CHIP) -DTRACE_LEVEL=$(TRACE_LEVEL)
ASFLAGS = -mcpu=cortex-a5 -Wall -g $(OPTIMIZATION) $(INCLUDES) -D$(CHIP) -D__ASSEMBLY__
LDFLAGS= -mcpu=cortex-a5 -Wl,--cref -Wl,--check-sections -Wl,--gc-sections -Wl,--entry=entry -Wl,--unresolved-symbols=report-all -Wl,--warn-common -Wl,--warn-section-align -Wl,--warn-unresolved-symbols
LD_OPTIONAL=-Wl,--print-gc-sections -Wl,--stats

#-------------------------------------------------------------------------------
#		Files
#-------------------------------------------------------------------------------

# Directories where source files can be found

VPATH += ../..

# Objects built from C source files
C_OBJECTS += main.o

# Append OBJ and BIN directories to output filename
OUTPUT := $(BIN)/$(OUTPUT)

#-------------------------------------------------------------------------------
#		Rules
#-------------------------------------------------------------------------------

all: $(BIN) $(OBJ) $(MEMORIES)

$(BIN) $(OBJ):
	mkdir $@

define RULES
C_OBJECTS_$(1) = $(addprefix $(OBJ)/$(1)_, $(C_OBJECTS))
ASM_OBJECTS_$(1) = $(addprefix $(OBJ)/$(1)_, $(ASM_OBJECTS))

$(1): $$(ASM_OBJECTS_$(1)) $$(C_OBJECTS_$(1))
	$(CC) $(LIB_PATH) $(LDFLAGS) $(LD_OPTIONAL) -T"$(BOARD_LIB)/resources/gcc/$(CHIP)/$$@.ld" -Wl,-Map,$(OUTPUT)-$$@.map -o $(OUTPUT)-$$@.elf $$^ $(LIBS)
	$(NM) $(OUTPUT)-$$@.elf >$(OUTPUT)-$$@.elf.txt
	$(OBJCOPY) -O binary $(OUTPUT)-$$@.elf $(OUTPUT)-$$@.bin
	$(SIZE) $$^ $(OUTPUT)-$$@.elf

$$(C_OBJECTS_$(1)): $(OBJ)/$(1)_%.o: %.c Makefile $(OBJ) $(BIN)
	$(CC) $(CFLAGS) -D$(1) -c -o $$@ $$<

$$(ASM_OBJECTS_$(1)): $(OBJ)/$(1)_%.o: %.S Makefile $(OBJ) $(BIN)
	$(CC) $(ASFLAGS) -D$(1) -c -o $$@ $$<

debug_$(1): $(1)
	$(GDB) -x "$(BOARD_LIB)/resources/gcc/sama5d4x-ek-$(1).gdb" -ex "reset" -readnow -se $(OUTPUT)-$(1).elf
endef

$(foreach MEMORY, $(MEMORIES), $(eval $(call RULES,$(MEMORY))))

clean:
	-cs-rm -fR $(OBJ) $(BIN)
