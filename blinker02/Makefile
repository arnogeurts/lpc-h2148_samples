
#ARMGNU=arm-thumb-elf
#ARMGNU=arm-none-linux-gnueabi
ARMGNU?=arm-none-eabi

LOPS = -Wall -m32 -emit-llvm

LLCOPS = -march=arm

COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding

AOPS = --warn --fatal-warnings

OOPS = -std-compile-opts

stuff = -O2 -std-compile-opts -strip-debug




all : blinker02.gcc.bin blinker02.clang.norm.bin blinker02.clang.opt.bin

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o

blinker02.gcc.o : blinker02.c
	$(ARMGNU)-gcc $(COPS) -c blinker02.c -o blinker02.gcc.o

blinker02.gcc.bin : memmap vectors.o blinker02.gcc.o
	$(ARMGNU)-ld -o blinker02.gcc.elf -T memmap vectors.o blinker02.gcc.o
	$(ARMGNU)-objdump -D blinker02.gcc.elf > blinker02.gcc.list
	$(ARMGNU)-objcopy blinker02.gcc.elf blinker02.gcc.bin -O binary

blinker02.clang.bc : blinker02.c
	clang $(LOPS) -c blinker02.c -o blinker02.clang.bc

blinker02.clang.norm.bin : memmap vectors.o blinker02.clang.bc
	llc $(LLCOPS) blinker02.clang.bc -o blinker02.clang.norm.s
	$(ARMGNU)-as blinker02.clang.norm.s -o blinker02.clang.norm.o
	$(ARMGNU)-ld -o blinker02.clang.norm.elf -T memmap vectors.o blinker02.clang.norm.o
	$(ARMGNU)-objdump -D blinker02.clang.norm.elf > blinker02.clang.norm.list
	$(ARMGNU)-objcopy blinker02.clang.norm.elf blinker02.clang.norm.bin -O binary

blinker02.clang.opt.bin : memmap vectors.o blinker02.clang.bc
	opt $(OOPS) blinker02.clang.bc -o blinker02.clang.opt.bc
	llc $(LLCOPS) blinker02.clang.opt.bc -o blinker02.clang.opt.s
	$(ARMGNU)-as blinker02.clang.opt.s -o blinker02.clang.opt.o
	$(ARMGNU)-ld -o blinker02.clang.opt.elf -T memmap vectors.o blinker02.clang.opt.o
	$(ARMGNU)-objdump -D blinker02.clang.opt.elf > blinker02.clang.opt.list
	$(ARMGNU)-objcopy blinker02.clang.opt.elf blinker02.clang.opt.bin -O binary


clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

