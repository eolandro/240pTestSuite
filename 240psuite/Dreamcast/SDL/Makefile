TARGET = main

OPTFLAGS=-O3 -fomit-frame-pointer -DDREAMCAST

KOS_CFLAGS+= -I$(KOS_BASE)/../kos-ports/include/SDL-1.2.9  $(OPTFLAGS)

all: $(TARGET).bin

include $(KOS_BASE)/Makefile.rules

.SRCS	=	main.c \

OBJS = $(.SRCS:.c=.o)

romdisk.img:
	$(KOS_GENROMFS) -f romdisk.img -d romdisk -v

romdisk.o: romdisk.img
	$(KOS_BASE)/utils/bin2o/bin2o romdisk.img romdisk romdisk.o

clean:
	rm -f $(OBJS) $(TARGET).elf $(TARGET).bin romdisk.o romdisk.img

$(TARGET).elf: $(OBJS) romdisk.o
	$(KOS_CC) $(KOS_CFLAGS) $(KOS_LDFLAGS) romdisk.o -o $(TARGET).elf $(KOS_START) \
	$(OBJS) -lSDL_gfx -lSDL_mixer -lSDL_129 -lm $(OBJEXTRA) $(KOS_LIBS)


$(TARGET).bin: $(TARGET).elf
	$(KOS_OBJCOPY) -R .stack -O binary $(TARGET).elf $(TARGET).bin
#	strip $(TARGET).elf

run: $(TARGET).bin
	$(KOS_LOADER) $(TARGET).bin

