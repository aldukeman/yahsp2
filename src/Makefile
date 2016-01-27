#!/bin/sh
#
# Makefile for CPT
#

MAKEFLAGS += -j3

CC     	= g++
AR	= ar
BISON   	= bison
FLEX    	= flex

EXEDIR = ../bin
LIBDIR = ../lib
LIBINCDIR = ../include

CFLAGS =  -O3 -Wall -g -fopenmp
EXE     	= $(EXEDIR)/cpt
MTEXE     	= $(EXEDIR)/cpt-mt
RATP     	= $(EXEDIR)/cpt-ratp
LIB      	= $(LIBDIR)/libcpt.a
MTLIB      	= $(LIBDIR)/libcpt-mt.a
LDLIBS   =  -lm -lgmp -fopenmp -lgdsl

FLEX_SOURCES = \
	parser.lex

BISON_SOURCES = \
	parser.y

SOURCES =  \
	pddl.c \
	instantiation.c \
	var.c \
	options.c \
	max_atom.c \
	problem.c \
	plan.c \
	preprocess.c \
	propagations.c \
	resources.c \
	scheduling.c \
	heuristics.c \
	branching.c \
	trace_planner.c \
	solve.c

YSOURCES = \
	yahsp-seq.c

MTYSOURCES = \
	yahsp-mt.c
  
EXESOURCES = \
	main.c

RATPSOURCES = \
	ratp.c

LIBSOURCES =  \


ALLSOURCES = $(BISON_SOURCES) $(FLEX_SOURCES) $(SOURCES) $(YSOURCES) $(MTYSOURCES) $(EXESOURCES) $(RATPSOURCES) $(LIBSOURCES)

BISON_GEN = $(BISON_SOURCES:.y=.tab.c)
FLEX_GEN =  $(FLEX_SOURCES:.lex=.yy.c)

OBJECTS = $(BISON_GEN:.c=.o)  $(FLEX_GEN:.c=.o) $(SOURCES:.c=.o) 
EXEOBJECTS = $(EXESOURCES:.c=.o) 
YOBJECTS = $(YSOURCES:.c=.o) 
MTYOBJECTS = $(MTYSOURCES:.c=.o) 
RATPOBJECTS = $(RATPSOURCES:.c=.o) 
LIBOBJECTS = $(LIBSOURCES:.c=.o) 

.PHONY : all pp ratp dep clean distclean win

all: CPPFLAGS += -DDAE
all: nodae

nodae: lib mtlib exe  mt

exe: $(EXE) 

mt:  $(MTEXE) 

lib: $(LIB)

mtlib: $(MTLIB)

ratp: CPPFLAGS += -DSCHED 
ratp: LDLIBS += -lX11
ratp: $(RATP) 

wct: CFLAGS += -DWALLCLOCK_TIME
wct: exe

$(EXE): $(OBJECTS) $(YOBJECTS) $(EXEOBJECTS)  
	$(CC) -o $@  $(OBJECTS) $(YOBJECTS) $(EXEOBJECTS) $(LDLIBS)

$(MTEXE): $(OBJECTS) $(MTYOBJECTS) $(EXEOBJECTS)  
	$(CC) -o $@  $(OBJECTS) $(MTYOBJECTS) $(EXEOBJECTS) $(LDLIBS)

$(RATP): $(OBJECTS) $(RATPOBJECTS)  
	$(CC) -o $@  $(OBJECTS) $(RATPOBJECTS) $(LDLIBS)

$(LIB): $(OBJECTS) $(YOBJECTS) $(LIBOBJECTS)
	$(AR) rcs $@ $(OBJECTS) $(YOBJECTS) $(LIBOBJECTS)

$(MTLIB): $(OBJECTS) $(MTYOBJECTS) $(LIBOBJECTS)
	$(AR) rcs $@ $(OBJECTS) $(MTYOBJECTS) $(LIBOBJECTS)

$(BISON_GEN): $(BISON_SOURCES)
	$(BISON) $(BISON_SOURCES)

$(FLEX_GEN): $(FLEX_SOURCES) $(BISON_GEN)
	$(FLEX) $(FLEX_SOURCES)

dep:
	makedepend -- $(ALLSOURCES) 

depr:
	makedepend -- -DSCHED -- $(ALLSOURCES) 

clean:
	rm -f $(EXE) $(MTEXE) $(LIB) $(MTLIB) $(BISON_GEN) $(FLEX_GEN) $(OBJECTS) $(YOBJECTS) $(MTYOBJECTS) $(EXEOBJECTS)  $(RATPOBJECTS) $(LIBOBJECTS) parser.tab.h *~ core
