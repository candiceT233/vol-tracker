#Your HDF5 install path
# HDF5_DIR=/home/mtang11/install/hdf5-1_13_1
# HDF5_DIR=/qfs/people/tang584/.conda/pkgs/hdf5-1.14.0-nompi_h5231ba7_103
# HDF5_DIR=/qfs/people/tang584/.conda/pkgs/hdf5-1.14.0-nompi_hb72d44e_103
# HDF5_DIR=$(HOME)/install/hdf5
HDF5_DIR=$(HOME)/.conda/envs/pyflextrkr_copy
# HDF5_DIR=/Users/koziol/HDF5/github/hpc-io/async_vol_register_optional/build_parallel_debug/hdf5

CC=mpicc
# CC=gcc
# CC=h5cc
AR=ar
# TRACKER_LOGGING TRACKER_PROV_LOGGING TRACKER_MORE_LOGGING TRACKER_PT_LOGGING TRACKER_TEST_LOGGING
DEBUG= -g -O0 -DDEBUG -gdwarf-4 -gdwarf-3 -gdwarf-2 #-DTRACKER_TEST_LOGGING -gdwarf-2, -gdwarf-3 or -gdwarf-4
INCLUDES=-I$(HDF5_DIR)/include 

CFLAGS = $(DEBUG) -fPIC $(INCLUDES) -w -DTRACKER_SCHEMA #-Wall 

LIBS=-L$(HDF5_DIR)/lib -lhdf5 -lz -lcrypto

# Uncomment this line MacOS builds:
# DYNLDFLAGS = $(DEBUG) -dynamiclib -current_version 1.0 -fPIC $(LIBS)
# Uncomment this line Linux builds:
DYNLDFLAGS = $(DEBUG) -shared -fPIC $(LIBS)
LDFLAGS = $(DEBUG) $(LIBS)
ARFLAGS = rs

# Shared library VOL connector
DYNSRC = tracker_vol_new.c # tracker_vol tracker_vol_new tracker_vol_int
DYNOBJ = $(DYNSRC:.c=.o)
# Uncomment this line Linux builds:
DYNLIB = libh5tracker.so #libh5tracker.so
# Uncomment this line MacOS builds:
# DYNLIB = libh5tracker.dylib
DYNDBG = libh5tracker.dylib.dSYM


# # Testcase section
# EXSRC = vpicio_uni_h5.c
# EXOBJ = $(EXSRC:.c=.o)
# EXEXE = $(EXSRC:.c=.exe)
# EXDBG = $(EXSRC:.c=.exe.dSYM)

all: $(DYNLIB) #$(EXEXE) 

$(EXEXE): $(EXSRC) $(STATLIB) $(DYNLIB)
	$(CC) $(CFLAGS) $^ -o $(EXEXE) $(LDFLAGS)

$(DYNLIB): $(DYNSRC)
	$(CC) $(CFLAGS) $(DYNLDFLAGS) $^ -o $@

.PHONY: clean all
clean:
	rm -rf $(DYNOBJ) $(DYNLIB) $(DYNDBG) \
            $(EXOBJ) $(EXEXE) $(EXDBG)
