#
# For building the softboundcets library
#

all: softboundcets_rt

CC = clang

CFLAGS=-g -Wall -pedantic  -D__SOFTBOUNDCETS_SPATIAL_TEMPORAL -O3 -fno-strict-aliasing
# CFLAGS=-Wall -pedantic  -D__SOFTBOUNDCETS_TEMPORAL -O3 -fno-strict-aliasing
ARFLAGS=-rcs

# If LLVM_GOLD is set, also build a library for use with LTO
#
# Note that the name of the library is hardcoded in the compiler. Thus, we call
# it the same, but put it in the lto/ subdirectory. To use it, pass
# -L/path/to/softboundcets-lib/lto to the compiler, and use -flto during
# compilation and linking
ifneq ($(LLVM_GOLD),)
all: softboundcets_rt_lto softboundmpx_rt_lto
endif

SRC = softboundcets-checks.c softboundcets.c softboundcets-wrappers.c
OBJ = $(SRC:%.c=%.o)

softboundcets_rt: $(SRC)
	$(CC) $(CFLAGS) -fPIC -c $^
	ar $(ARFLAGS) lib$@.a $(OBJ)
	clang -fPIC -shared -o lib$@.so $(OBJ)

softboundcets_rt_lto: $(SRC)
	mkdir lto
	clang $(CFLAGS) -flto -c $^
	ar --plugin=$(LLVM_GOLD) $(ARFLAGS) lto/libsoftboundcets_rt.a lto/softboundcets.lto.o lto/softboundcets-checks.lto.o lto/softboundcets-wrappers.lto.o


clean:
	rm -rf *.o *.a *~ lto/

