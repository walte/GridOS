include $(MY_BUILD_BASE)/config.mk
include hal_include.mk

ADD_INCLUDE +=$(HAL_INCLUDE)

TARGET					= hal.sys
LDFLAGS					=-L$(SYSTEM_DIR) --entry=start 
DEPENDS 				= --warn-unresolved-symbols
ADD_LDFLAGS				+= -T arch/$(ARCH_DIR)/lds$(BITS).txt

DEPENDS					+= $(SYSTEM_BUILD_TMP)/libs/elf2/$(ARCH_DIR)/elf.o  ../../libs/common/*.o ../../kernel/*.a ../../subsystem/*.o
#要编译的文件列表
OBJS	= 
PARTS	= irq main time libs arch console


all:  subs_parts $(OBJS)
	$(MAKE) link
	
link:
	$(LD) $(LDFLAGS) -o $(TARGET) $(ADD_LDFLAGS) $(DEPENDS) *.o $(ARCH_LD_FLAGS)

#
#i386? we can test in virtual machine
#
ifeq ($(ARCH),i386)
	$(CP) $(TARGET) $(TARGET).org
	$(OBJCOPY) -O binary -S $(TARGET)
	$(CP) $(TARGET) $(SYSTEM_DIR)
else ifeq ($(ARCH),arm)
	$(CP) $(TARGET) $(TARGET).org
	$(OBJCOPY) -O binary -S $(TARGET)
	$(CP) $(TARGET) /private/tftpboot/C0A80163.img
endif

include $(MY_BUILD_BASE)/cc.mk
