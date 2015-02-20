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
##  Tested with the following mingw compilers distrubuted by             ##
##  http://rubyinstaller.org/downloads/                                  ##
##   * DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe                     ##
##   * DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe                     ##
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

LOCAL_CLEAN = $(DLLS:%=$(BINDIR)/%) $(DLLS:%=$(LIBDIR)/lib%.a) $(DLLS:.dll=.def)

ALL = $(DLLS:%=$(BINDIR)/%)

include $(TOP)/config/common_make_rules

VPATH = $(LIBDIR)

VOICE_DEP = libflite.dll.a libflite_usenglish.dll.a libflite_cmulex.dll.a

LINK_DLL = basename=`basename $@ .dll` && \
	$(DLLTOOL) -z $$basename.def --export-all-symbol $< && \
	rm -rf flitedll && \
	mkdir flitedll && \
	(cd flitedll && ar x ../$<) && \
	$(DLLWRAP) -def $$basename.def flitedll/*.o $(filter %.dll.a,$^) --output-lib $(LIBDIR)/lib$$basename.dll.a -o $@ $(if $(filter %/flite.dll,$@),-lwinmm) && \
	rm -rf flitedll

install: $(ALL)
	@ for i in $(DLLS:.dll=) ; \
	do \
	   $(INSTALL) $(BINDIR)/$$i.dll $(DESTDIR)$(INSTALLBINDIR); \
	   $(INSTALL) $(LIBDIR)/lib$$i.dll.a $(DESTDIR)$(INSTALLLIBDIR); \
	done

$(BINDIR)/flite.dll: libflite.a
	$(LINK_DLL)

$(BINDIR)/flite_cmu_grapheme_lang.dll: libflite_cmu_grapheme_lang.a libflite.dll.a
	$(LINK_DLL)

$(BINDIR)/flite_cmu_grapheme_lex.dll: libflite_cmu_grapheme_lex.a libflite.dll.a
	$(LINK_DLL)

$(BINDIR)/flite_cmu_indic_lang.dll: libflite_cmu_indic_lang.a libflite.dll.a libflite_usenglish.dll.a
	$(LINK_DLL)

$(BINDIR)/flite_cmu_indic_lex.dll: libflite_cmu_indic_lex.a libflite.dll.a libflite_cmu_indic_lang.dll.a libflite_cmulex.dll.a
	$(LINK_DLL)

$(BINDIR)/flite_cmu_time_awb.dll: libflite_cmu_time_awb.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmu_us_awb.dll: libflite_cmu_us_awb.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmu_us_kal.dll: libflite_cmu_us_kal.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmu_us_kal16.dll: libflite_cmu_us_kal16.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmu_us_rms.dll: libflite_cmu_us_rms.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmu_us_slt.dll: libflite_cmu_us_slt.a $(VOICE_DEP)
	$(LINK_DLL)

$(BINDIR)/flite_cmulex.dll: libflite_cmulex.a libflite.dll.a
	$(LINK_DLL)

$(BINDIR)/flite_usenglish.dll: libflite_usenglish.a libflite.dll.a
	$(LINK_DLL)

$(LIBDIR)/libflite.dll.a: $(BINDIR)/flite.dll
$(LIBDIR)/libflite_usenglish.dll.a: $(BINDIR)/flite_usenglish.dll
$(LIBDIR)/libflite_cmu_indic_lang.dll.a: $(BINDIR)/flite_cmu_indic_lang.dll
$(LIBDIR)/libflite_cmulex.dll.a: $(BINDIR)/flite_cmulex.dll
