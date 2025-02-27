#
# Makefile for the FREAX-kernel.
#
# Note! Dependencies are done automagically by 'make dep', which also
# removes any old dependencies. DON'T put your own dependencies here
# unless it's something special (ie not a .c file).
#

AR	=ar
AS	=as
LD	=ld
LDFLAGS	=-s -x
CC	=gcc
CFLAGS = -std=gnu89 -m32 -ffreestanding -Wall -O -fstrength-reduce -fomit-frame-pointer \
	-finline-functions -nostdinc -I../include
CPP	=gcc -E -nostdinc -I../include

.c.s:
	$(CC) $(CFLAGS) \
	-S -o $*.s $<
.s.o:
	$(AS) --32 -o $*.o $<
.c.o:
	$(CC) $(CFLAGS) \
	-c -o $*.o $<

OBJS  = sched.o system_call.o traps.o asm.o fork.o \
	panic.o printk.o vsprintf.o tty_io.o console.o \
	keyboard.o rs_io.o hd.o sys.o exit.o serial.o \
	mktime.o

kernel.o: $(OBJS)
	$(LD) -m elf_i386 -r -o kernel.o $(OBJS)
	sync

clean:
	rm -f core *.o *.a tmp_make
	for i in *.c;do rm -f `basename $$i .c`.s;done

dep:
	sed '/\#\#\# Dependencies/q' < Makefile > tmp_make
	(for i in *.c;do echo -n `echo $$i | sed 's,\.c,\.s,'`" "; \
		$(CPP) -M $$i;done) >> tmp_make
	cp tmp_make Makefile

### Dependencies:
console.s console.o : console.c ../include/linux/sched.h ../include/linux/head.h \
  ../include/linux/fs.h ../include/sys/types.h ../include/linux/mm.h \
  ../include/linux/tty.h ../include/termios.h ../include/asm/io.h \
  ../include/asm/system.h 
exit.s exit.o : exit.c ../include/errno.h ../include/signal.h \
  ../include/sys/types.h ../include/sys/wait.h ../include/linux/sched.h \
  ../include/linux/head.h ../include/linux/fs.h ../include/linux/mm.h \
  ../include/linux/kernel.h ../include/linux/tty.h ../include/termios.h \
  ../include/asm/segment.h 
fork.s fork.o : fork.c ../include/errno.h ../include/linux/sched.h \
  ../include/linux/head.h ../include/linux/fs.h ../include/sys/types.h \
  ../include/linux/mm.h ../include/linux/kernel.h ../include/asm/segment.h \
  ../include/asm/system.h 
hd.s hd.o : hd.c ../include/linux/config.h ../include/linux/sched.h \
  ../include/linux/head.h ../include/linux/fs.h ../include/sys/types.h \
  ../include/linux/mm.h ../include/linux/kernel.h ../include/linux/hdreg.h \
  ../include/asm/system.h ../include/asm/io.h ../include/asm/segment.h 
mktime.s mktime.o : mktime.c ../include/time.h 
panic.s panic.o : panic.c ../include/linux/kernel.h 
printk.s printk.o : printk.c ../include/stdarg.h ../include/stddef.h \
  ../include/linux/kernel.h 
sched.s sched.o : sched.c ../include/linux/sched.h ../include/linux/head.h \
  ../include/linux/fs.h ../include/sys/types.h ../include/linux/mm.h \
  ../include/linux/kernel.h ../include/signal.h ../include/linux/sys.h \
  ../include/asm/system.h ../include/asm/io.h ../include/asm/segment.h 
serial.s serial.o : serial.c ../include/linux/tty.h ../include/termios.h \
  ../include/linux/sched.h ../include/linux/head.h ../include/linux/fs.h \
  ../include/sys/types.h ../include/linux/mm.h ../include/asm/system.h \
  ../include/asm/io.h 
sys.s sys.o : sys.c ../include/errno.h ../include/linux/sched.h \
  ../include/linux/head.h ../include/linux/fs.h ../include/sys/types.h \
  ../include/linux/mm.h ../include/linux/tty.h ../include/termios.h \
  ../include/linux/kernel.h ../include/asm/segment.h ../include/sys/times.h \
  ../include/sys/utsname.h 
traps.s traps.o : traps.c ../include/string.h ../include/linux/head.h \
  ../include/linux/sched.h ../include/linux/fs.h ../include/sys/types.h \
  ../include/linux/mm.h ../include/linux/kernel.h ../include/asm/system.h \
  ../include/asm/segment.h 
tty_io.s tty_io.o : tty_io.c ../include/ctype.h ../include/errno.h \
  ../include/signal.h ../include/sys/types.h ../include/linux/sched.h \
  ../include/linux/head.h ../include/linux/fs.h ../include/linux/mm.h \
  ../include/linux/tty.h ../include/termios.h ../include/asm/segment.h \
  ../include/asm/system.h 
vsprintf.s vsprintf.o : vsprintf.c ../include/stdarg.h ../include/string.h 
