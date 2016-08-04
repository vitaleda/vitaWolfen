TARGET		:= vitaWolfen
WMODE		:= 1
TITLE		:= WOLF00001

LIBS = -lvita2d -lSceAppmgr_stub -lSceDisplay_stub -lSceGxm_stub	\
	-lSceSysmodule_stub -lSceCtrl_stub -lm \
	-lScePgf_stub -ljpeg -lfreetype -lc \
	-lSceCommonDialog_stub -lpng16 -lz

COMMON_OBJS = source/main.o source/font.o source/font_data.o

CFILES		:=	$(COMMON_OBJS)
CPPFILES   := $(foreach dir,$(SOURCES), $(wildcard $(dir)/*.cpp))
BINFILES := $(foreach dir,$(DATA), $(wildcard $(dir)/*.bin))
OBJS     := $(addsuffix .o,$(BINFILES)) $(CFILES:.c=.o) $(CPPFILES:.cpp=.o) 
			
PREFIX  = arm-vita-eabi
CC      = $(PREFIX)-gcc
CXX      = $(PREFIX)-g++
CFLAGS  = -Wl,-q -O3
CXXFLAGS  = $(CFLAGS) -fno-exceptions
ASFLAGS = $(CFLAGS)

all: $(TARGET).vpk

$(TARGET).vpk: $(TARGET).velf
	vita-make-fself $< ..\release\eboot.bin
	vita-mksfoex -s TITLE_ID=$(TITLE) "$(TARGET)" param.sfo
	cp -f param.sfo ../release/sce_sys/param.sfo
	
	#------------ Comment this if you don't have 7zip ------------------
	7z a -tzip ./$(TARGET).vpk -r ..\release\sce_sys\* ..\release\eboot1.bin ..\release\eboot2.bin ..\release\eboot3.bin ..\release\eboot0.bin ..\release\eboot.bin 
	#-------------------------------------------------------------------
	
%.velf: %.elf
	$(PREFIX)-strip -g $<
	vita-elf-create $< $@

$(TARGET).elf: $(OBJS)
	$(CXX) $(CXXFLAGS) $^ $(LIBS) -o $@

clean:
	@rm -rf $(TARGET).velf $(TARGET).elf $(OBJS) param.sfo eboot.bin