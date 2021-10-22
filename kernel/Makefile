
OSNAME = eOS

LDS = linker.ld
CC = 	i686-elf-gcc
LD = i686-elf-ld
AS = i686-elf-as

CFLAGS = -std=gnu99 -ffreestanding -O2 -Wall -Wextra
LDFLAGS = -ffreestanding -O2 -nostdlib

SRCDIR := src
OBJDIR := lib
BUILDDIR := bin
ISODIR := ../isodir
OUTDIR := ../out

OUTISONAME := eOS.iso

rwildcard=$(foreach d, $(wildcard $(1:=/*)), $(call rwildcard,$d,$2) $(filter $(subst *,%,$2), $d))

SRC = $(call rwildcard,$(SRCDIR),*.c)
OBJS = $(patsubst $(SRCDIR)/%.c, $(OBJDIR)/%.o, $(SRC))
DIRS = $(wildcard $(SRCDIR)/*)

all: boot kernel buildimg

kernel: $(OBJS) link

$(OBJDIR)/%.o: $(SRCDIR)/%.c
	@echo !==== COMPILING $^
	@mkdir -p $(@D)
	$(CC) -c $^ -o $@ $(CFLAGS)

link:
	@echo !==== LINKING
	$(CC) -T $(LDS) -o $(BUILDDIR)/kernel.bin $(LDFLAGS) $(OBJS) $(OBJDIR)/boot.o -lgcc

setup:
	@mkdir $(OBJDIR)
	@mkdir $(SRCDIR)
	@mkdir $(BUILDDIR)

buildimg:
	@cp $(BUILDDIR)/kernel.bin $(ISODIR)/boot/kernel.bin
	@cp grub.cfg $(ISODIR)/boot/grub/grub.cfg
	@grub-mkrescue -o $(OUTDIR)/$(OUTISONAME) $(ISODIR)

boot:
	$(AS) $(SRCDIR)/boot/boot.s -o $(OBJDIR)/boot.o

run:
	qemu-system-i386 -cdrom $(OUTDIR)/$(OUTISONAME)
