###########################################################################
##                                                                       ##
##                  Language Technologies Institute                      ##
##                     Carnegie Mellon University                        ##
##                         Copyright (c) 2008                            ##
##                        All Rights Reserved.                           ##
##                                                                       ##
##  Permission is hereby granted, free of charge, to use and distribute  ##
##  this software and its documentation without restriction, including   ##
##  without limitation the rights to use, copy, modify, merge, publish,  ##
##  distribute, sublicense, and/or sell copies of this work, and to      ##
##  permit persons to whom this work is furnished to do so, subject to   ##
##  the following conditions:                                            ##
##   1. The code must retain the above copyright notice, this list of    ##
##      conditions and the following disclaimer.                         ##
##   2. Any modifications must be clearly marked as such.                ##
##   3. Original authors' names are not deleted.                         ##
##   4. The authors' names are not used to endorse or promote products   ##
##      derived from this software without specific prior written        ##
##      permission.                                                      ##
##                                                                       ##
##  CARNEGIE MELLON UNIVERSITY AND THE CONTRIBUTORS TO THIS WORK         ##
##  DISCLAIM ALL WARRANTIES WITH REGARD TO THIS SOFTWARE, INCLUDING      ##
##  ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO EVENT   ##
##  SHALL CARNEGIE MELLON UNIVERSITY NOR THE CONTRIBUTORS BE LIABLE      ##
##  FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES    ##
##  WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN   ##
##  AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION,          ##
##  ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF       ##
##  THIS SOFTWARE.                                                       ##
##                                                                       ##
###########################################################################
##                                                                       ##
##  Windows support for Flite                                            ##
##                                                                       ##
##  Builds a usable flite, language and voice .dlls with mingw           ##
##                                                                       ##
##     http://www.mingw.org                                              ##
##                                                                       ##
##  At top level                                                         ##
##      ./configure --enablle-shared                                     ##
##      make                                                             ##
##                                                                       ##
##  '--enablle-shared' is required to create dll and lib flies.          ##
##                                                                       ##
###########################################################################
TOP = ..
DIRNAME = windows
BUILD_DIRS = 
ALL_DIRS = $(BUILD_DIRS) 
H = 
RC = 
SRCS = 
OBJS = $(SRCS:.c=.o)
FILES = Makefile $(SRCS) $(H) $(RC) $(WINDOWS)

DLLS = flite.dll \
       flite_cmu_grapheme_lang.dll \
       flite_cmu_grapheme_lex.dll \
       flite_cmu_indic_lang.dll \
       flite_cmu_indic_lex.dll \
       flite_cmu_time_awb.dll \
       flite_cmu_us_awb.dll \
       flite_cmu_us_kal.dll \
       flite_cmu_us_kal16.dll \
       flite_cmu_us_rms.dll \
       flite_cmu_us_slt.dll \
       flite_cmulex.dll \
       flite_usenglish.dll

LOCAL_CLEAN = $(DLLS) $(DLLS:.dll=.lib) $(DLLS:.dll=.def)

ALL = $(DLLS)

include $(TOP)/config/common_make_rules

VPATH = $(LIBDIR)

%.dll: lib%.a
	$(DLLTOOL) -z $*.def --export-all-symbol $<
	rm -rf flitedll
	mkdir flitedll
	(cd flitedll && ar x ../$<)
	$(DLLWRAP) -def $*.def flitedll/*.o $(filter %.lib,$^) --output-lib $*.lib -o $@ $(if $(filter flite.dll,$@),-lwinmm)
	rm -rf flitedll

install: $(DLLS)
	@ for i in $(DLLS:.dll=) ; \
	do \
	   $(INSTALL) $$i.dll $(DESTDIR)$(INSTALLBINDIR); \
	   $(INSTALL) $$i.lib $(DESTDIR)$(INSTALLLIBDIR); \
	done

flite.dll: libflite.a
flite_cmu_grapheme_lang.dll: libflite_cmu_grapheme_lang.a flite.lib
flite_cmu_grapheme_lex.dll: libflite_cmu_grapheme_lex.a flite.lib
flite_cmu_indic_lang.dll: libflite_cmu_indic_lang.a flite.lib flite_usenglish.lib
flite_cmu_indic_lex.dll: libflite_cmu_indic_lex.a flite.lib flite_cmu_indic_lang.lib flite_cmulex.lib
flite_cmu_time_awb.dll: libflite_cmu_time_awb.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmu_us_awb.dll: libflite_cmu_us_awb.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmu_us_kal.dll: libflite_cmu_us_kal.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmu_us_kal16.dll: libflite_cmu_us_kal16.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmu_us_rms.dll: libflite_cmu_us_rms.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmu_us_slt.dll: libflite_cmu_us_slt.a flite.lib flite_usenglish.lib flite_cmulex.lib
flite_cmulex.dll: libflite_cmulex.a flite.lib
flite_usenglish.dll: libflite_usenglish.a flite.lib

flite.lib: flite.dll
flite_usenglish.lib: flite_usenglish.dll
flite_cmu_indic_lang.lib: flite_cmu_indic_lang.dll
flite_cmulex.lib: flite_cmulex.dll
