PREFIX := arm-none-eabi-

CFLAGS += -mcpu=cortex-m4 -mthumb -g3 -o0
LDFLAGS += ${CFLAGS} -nostartfiles -nodefaultlibs -Wl,-Tlinker.ld

all: build.elf build.hex

main.o: main.c
	${PREFIX}gcc ${CFLAGS} -c main.c -o main.o

startup.o: startup.c
	${PREFIX}gcc ${CFLAGS} -c startup.c -o startup.o

build.elf: main.o startup.o linker.ld
	${PREFIX}gcc ${LDFLAGS} main.o startup.o  -o build.elf

build.hex: build.elf
	${PREFIX}objcopy -O ihex build.elf build.hex

openocd:
	/scratch/opt/openocd0100/bin/openocd \
		-f interface/stlink-v2.cfg  \
		-f target/nrf52.cfg  \
		-c "telnet_port pipe; tcl_port disabled; gdb_port disabled; log_output /dev/null"

.PHONY: clean

clean:
	rm build.elf *.o
