include prorab.mk


this_name := tests


this_srcs := main.cpp


this_cflags := -Wall
this_cflags += -Wno-comment #no warnings on nested comments
this_cflags += -Wno-format
this_cflags += -fstrict-aliasing #strict aliasing!!!
this_cflags += -g
this_cflags += -std=c++11

ifeq ($(debug), true)
    this_cflags += -DDEBUG
endif

ifeq ($(prorab_os),windows)
#    this_ldlibs += -lmingw32 #these should go first, otherwise linker will complain about undefined reference to WinMain
#    this_ldlibs += $(prorab_this_dir)../../src/libaudout$(prorab_lib_extension)
#    this_ldflags += -L/usr/lib -L/usr/local/lib
#    this_ldlibs +=  -lglew32 -lopengl32 -lpng -ljpeg -lz -lfreetype -mwindows

#    this_cflags += -I/usr/include -I/usr/local/include

    #WORKAROUND for MinGW bug:
#    this_cflags += -D__STDC_FORMAT_MACROS
else ifeq ($(prorab_os),macosx)
#    this_ldlibs += $(prorab_this_dir)../../src/libaudout$(prorab_lib_extension) -lGLEW -framework OpenGL -framework Cocoa -lpng -ljpeg -lfreetype
else ifeq ($(prorab_os),linux)
    this_ldlibs += $(prorab_this_dir)../../src/libaudout$(prorab_lib_extension)
endif

this_ldlibs += -lpogodi -lnitki -lpthread -lpulse -lpulse-simple #-lasound


$(eval $(prorab-build-app))


define this_rules
test:: $(prorab_this_name)
	@echo running $$^...
	@(cd $(prorab_this_dir); LD_LIBRARY_PATH=../../src $$^)
endef
$(eval $(this_rules))


#add dependency on libaudout
ifeq ($(prorab_os),windows)
    $(prorab_this_dir)libaudout$(prorab_lib_extension): $(abspath $(prorab_this_dir)../../src/libaudout$(prorab_lib_extension))
	@cp $< $@

    $(prorab_this_name): $(prorab_this_dir)libaudout$(prorab_lib_extension)

    define this_rules
        clean::
		@rm -f $(prorab_this_dir)libaudout$(prorab_lib_extension)
    endef
    $(eval $(this_rules))
else
    $(prorab_this_name): $(abspath $(prorab_this_dir)../../src/libaudout$(prorab_lib_extension))
endif


$(eval $(call prorab-include,$(prorab_this_dir)../../src/makefile))