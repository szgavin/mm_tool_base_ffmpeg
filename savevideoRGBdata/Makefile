# use pkg-config for getting CFLAGS and LDLIBS
FFMPEG_LIBS=    libavdevice                        \
                libavformat                        \
                libavfilter                        \
                libavcodec                         \
                libswresample                      \
                libswscale                         \
                libavutil                          \

CFLAGS += -Wall -g
CFLAGS := $(shell pkg-config --cflags $(FFMPEG_LIBS)) $(CFLAGS)
LDLIBS := $(shell pkg-config --libs $(FFMPEG_LIBS)) $(LDLIBS)

EXAMPLES=       tutoeial01
OBJS=$(addsuffix .o,$(EXAMPLES))

# the following examples make explicit use of the math library
avcodec:           LDLIBS += -lm
encode_audio:      LDLIBS += -lm
muxing:            LDLIBS += -lm
resampling_audio:  LDLIBS += -lm

.phony: all clean

all: $(OBJS) $(EXAMPLES)

clean: 
	$(RM) $(EXAMPLES) $(OBJS)

