#
# For building the softboundcets library
#

BIN = ../../build/bin
CC = $(BIN)/clang
CFLAGS= -Wall -pedantic  -D__SOFTBOUNDCETS_TEMPORAL -O3 -fno-strict-aliasing \
		-Wno-unused-variable
AR=$(BIN)/llvm-ar
ARFLAGS=-rcs

all: softboundcets_rt
lto: softboundcets_rt_lto

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
	$(AR) $(ARFLAGS) lib$@.a $(OBJ)
	$(CC) -fPIC -shared -o lib$@.so $(OBJ)

softboundcets_rt_lto: $(SRC)
	rm -r lto; mkdir lto
	$(CC) $(CFLAGS) -flto -c $^
	mv $(OBJ) lto/
	$(AR) $(ARFLAGS) lto/libsoftboundcets_rt.a lto/*.o


clean:
	rm -rf *.o *.a *~ lto/

