TARGET		:= smworld
TITLE	:= SPRMRWRLD
SOURCES	:= ../.. ../../snes ../../../smb1 ../../../smbll

LIBS = -lSDL2_mixer -lSDL2_image -lSDL2 -lpng -lwebp -ljpeg -lmikmod -lz -lm -lvita2d \
  -lstdc++ -lsndfile -lvorbis -lvorbisenc -lvorbisfile -logg -lcurl -lssl -lSceIme_stub \
  -lcrypto -lFLAC -lopenal -limgui -lvitaGL -lvitashark -lSceShaccCgExt -lmathneon \
  -lmpg123 -ltaihen_stub -lkubridge_stub -lSceAppMgr_stub -lSceAppUtil_stub -lSceAudio_stub \
  -lSceAudioIn_stub -lSceCtrl_stub -lSceCommonDialog_stub -lSceDisplay_stub -lSceFios2_stub \
  -lSceGxm_stub -lSceLibcBridge_stub -lSceShaccCg_stub -lSceSysmodule_stub -lScePower_stub \
  -lSceTouch_stub -lSceMotion_stub -lSceHid_stub -lSceVshBridge_stub -lSceKernelDmacmgr_stub

CFILES   := $(foreach dir,$(SOURCES), $(wildcard $(dir)/*.c))
CPPFILES   := $(foreach dir,$(SOURCES), $(wildcard $(dir)/*.cpp))
BINFILES := $(foreach dir,$(DATA), $(wildcard $(dir)/*.bin))
OBJS     := $(addsuffix .o,$(BINFILES)) $(CFILES:.c=.o) $(CPPFILES:.cpp=.o) 

PREFIX  = arm-vita-eabi
CC      = $(PREFIX)-gcc
CXX      = $(PREFIX)-g++
CFLAGS  = -g -Wl,-q,--wrap,fopen,--wrap,mkdir -O3 -ffast-math -DSTBI_NO_THREAD_LOCALS \
	-I$(VITASDK)/$(PREFIX)/include/SDL2 -fno-tree-vectorize -DNDEBUG -I../../..
CXXFLAGS  = $(CFLAGS) -fno-exceptions -std=gnu++11 -fpermissive
ASFLAGS = $(CFLAGS)

all: $(TARGET).vpk

$(TARGET).vpk: $(TARGET).velf
	vita-make-fself -c -s $< eboot.bin
	vita-mksfoex -s TITLE_ID=$(TITLE) "Super Mario World" param.sfo
	vita-pack-vpk -s param.sfo -b eboot.bin \
		--add icon0.png=sce_sys/icon0.png \
		--add bg.png=sce_sys/livearea/contents/bg.png \
		--add startup.png=sce_sys/livearea/contents/startup.png \
		--add template.xml=sce_sys/livearea/contents/template.xml \
		--add bg_sel.png=bg_sel.png \
		$(TARGET).vpk

%.velf: %.elf
	cp $< $<.unstripped.elf
	$(PREFIX)-strip -g $<
	vita-elf-create $< $@

$(TARGET).elf: $(OBJS)
	$(CXX) $(CXXFLAGS) $^ $(LIBS) -o $@

clean:
	@rm -rf $(TARGET).velf $(TARGET).elf $(TARGET).sysvelf $(OBJS)
