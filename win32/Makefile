# Oniguruma Makefile for Win32

product_name = oniguruma

# Comment out the next line, if you don't want to use LTCG.
USE_LTCG = 1

# Enable the next line, if you want to use a .def file.
#DEFFILE = onig.def


CPPFLAGS = 
CFLAGS = -O2 -nologo /W3
LDFLAGS =
LOADLIBES =
ARLIB = lib
ARLIB_FLAGS = -nologo
ARDLL = cl
ARDLL_FLAGS = -nologo -LD $(LINKFLAGS) -dll
LINKFLAGS = -link -incremental:no -pdb:none

INSTALL = install -c
CP      = copy
CC = cl
RC = rc
DEFS = -DHAVE_CONFIG_H -DNOT_RUBY -DEXPORT
RUBYDIR = ..
PYTHON = py

# Command string to get the version of cl.exe
_MSC_VER = [for /f %i in ('cmd /c "(echo _MSC_VER>mscver.c) && ($(CC) /EP mscver.c 2>nul) && del mscver.c"') do @exit %i]

!if DEFINED(USE_LTCG) && $(USE_LTCG)
# Use LTCG (Link Time Code Generation).
# Check if cl.exe is newer than VC++ 7.0 (_MSC_VER >= 1300).
!if $(_MSC_VER) >= 1300
CFLAGS = $(CFLAGS) -GL
ARLIB_FLAGS = $(ARLIB_FLAGS) -LTCG
ARDLL_FLAGS = $(ARDLL_FLAGS) -LTCG
!endif
!endif

!ifdef DEFFILE
# Use a .def file to export APIs.
CFLAGS = $(CFLAGS) /DONIG_EXTERN=extern
ARDLL_FLAGS = $(ARDLL_FLAGS) -def:$(DEFFILE)
!endif

subdirs = 

libbase   = onig
libname   = $(libbase)_s.lib
dllname   = $(libbase).dll
dlllib    = $(libbase).lib

onigheaders  = oniguruma.h regint.h regparse.h regenc.h st.h
posixheaders = onigposix.h
headers      = $(posixheaders) $(onigheaders)

onigobjs     = reggnu.obj regerror.obj regparse.obj regext.obj regcomp.obj \
	       regexec.obj regenc.obj regsyntax.obj regtrav.obj \
	       regversion.obj st.obj
posixobjs    = regposix.obj regposerr.obj
libobjs      = $(onigobjs) $(posixobjs)

jp_objs      =  $(encdir)\euc_jp.obj $(encdir)\sjis.obj $(encdir)\cp932.obj
iso8859_objs =  $(encdir)\iso8859_1.obj  $(encdir)\iso8859_2.obj \
		$(encdir)\iso8859_3.obj  $(encdir)\iso8859_4.obj \
		$(encdir)\iso8859_5.obj  $(encdir)\iso8859_6.obj \
		$(encdir)\iso8859_7.obj  $(encdir)\iso8859_8.obj \
		$(encdir)\iso8859_9.obj  $(encdir)\iso8859_10.obj \
		$(encdir)\iso8859_11.obj $(encdir)\iso8859_13.obj \
		$(encdir)\iso8859_14.obj $(encdir)\iso8859_15.obj \
		$(encdir)\iso8859_16.obj

encobjs      =  $(encdir)\ascii.obj $(encdir)\utf8.obj \
		$(encdir)\unicode.obj \
		$(encdir)\utf16_be.obj $(encdir)\utf16_le.obj \
		$(encdir)\utf32_be.obj $(encdir)\utf32_le.obj \
		$(jp_objs) $(iso8859_objs) \
		$(encdir)\euc_tw.obj $(encdir)\euc_kr.obj $(encdir)\big5.obj \
		$(encdir)\gb18030.obj \
		$(encdir)\koi8_r.obj  \
		$(encdir)\cp1251.obj # $(encdir)\koi8.obj

resobj       = win32\onig.res

onigsources  = regerror.c regparse.c regext.c regcomp.c regexec.c regenc.c \
	       regsyntax.c regtrav.c regversion.c reggnu.c st.c
posixsources = regposix.c regposerr.c
libsources   = $(posixsources) $(onigsources)
rubysources  = $(onigsources)

encdir       = enc
patchfiles   = re.c.168.patch re.c.181.patch
distfiles    = README COPYING HISTORY \
		Makefile.in configure.in config.h.in configure \
		$(headers) $(libsources) $(patchfiles) \
		test.rb testconv.rb
testc        = testc
testcu       = testu
testp        = testp

makeargs = $(MFLAGS) CPPFLAGS='$(CPPFLAGS)' CFLAGS='$(CFLAGS)' CC='$(CC)'

.SUFFIXES:
.SUFFIXES: .obj .c .h .ps .dvi .info .texinfo .res .rc

!ifdef NOBatch
.c.obj:
	$(CC) $(CFLAGS) $(CPPFLAGS) $(DEFS) /I. /I.. /Fo$@ /c $<
!else
# batch-mode inference rules
.c.obj::
	$(CC) $(CFLAGS) $(CPPFLAGS) $(DEFS) /I. /Fo.\ /c $<
{$(encdir)\}.c{$(encdir)\}.obj::
	$(CC) $(CFLAGS) $(CPPFLAGS) $(DEFS) /I. /I.. /Fo$(encdir)\ /c $<
!endif

.rc.res:
	$(RC) /Fo$@ $<

# targets
default: all

setup:
	$(CP) win32\config.h config.h
	$(CP) win32\testc.c  testc.c


all: $(libname) $(dllname) 

$(libname): $(libobjs) $(encobjs)
	$(ARLIB) $(ARLIB_FLAGS) -out:$@ $(libobjs) $(encobjs)

$(dllname): $(libobjs) $(encobjs) $(resobj) $(DEFFILE)
	$(ARDLL) $(libobjs) $(encobjs) $(resobj) -Fe$@ $(ARDLL_FLAGS)

regparse.obj:  regparse.c $(onigheaders) config.h st.h
regext.obj:    regext.c   $(onigheaders) config.h
regtrav.obj:   regtrav.c  $(onigheaders) config.h
regcomp.obj:   regcomp.c  $(onigheaders) config.h
regexec.obj:   regexec.c  regint.h regenc.h oniguruma.h config.h
reggnu.obj:    reggnu.c   regint.h regenc.h oniguruma.h config.h oniggnu.h
regerror.obj:  regerror.c regint.h regenc.h oniguruma.h config.h
regenc.obj:    regenc.c   regint.h regenc.h oniguruma.h config.h
regsyntax.obj: regsyntax.c regint.h regenc.h oniguruma.h config.h
regversion.obj: regversion.c oniguruma.h config.h
regposix.obj:  regposix.c $(posixheaders) oniguruma.h config.h
regposerr.obj: regposerr.c $(posixheaders) config.h
st.obj:        st.c regint.h oniguruma.h config.h st.h

$(encdir)\ascii.obj:      $(encdir)\ascii.c regenc.h config.h
$(encdir)\unicode.obj:    $(encdir)\unicode.c regint.h regenc.h config.h $(encdir)\unicode\casefold.h $(encdir)\unicode\name2ctype.h
$(encdir)\utf8.obj:       $(encdir)\utf8.c regenc.h config.h
$(encdir)\utf16_be.obj:   $(encdir)\utf16_be.c regenc.h config.h
$(encdir)\utf16_le.obj:   $(encdir)\utf16_le.c regenc.h config.h
$(encdir)\utf32_be.obj:   $(encdir)\utf32_be.c regenc.h config.h
$(encdir)\utf32_le.obj:   $(encdir)\utf32_le.c regenc.h config.h
$(encdir)\euc_jp.obj:     $(encdir)\euc_jp.c regenc.h config.h
$(encdir)\euc_tw.obj:     $(encdir)\euc_tw.c regenc.h config.h
$(encdir)\euc_kr.obj:     $(encdir)\euc_kr.c regenc.h config.h
$(encdir)\sjis.obj:       $(encdir)\sjis.c regenc.h config.h
$(encdir)\cp932.obj:      $(encdir)\cp932.c $(encdir)\sjis.c regenc.h config.h
$(encdir)\iso8859_1.obj:  $(encdir)\iso8859_1.c regenc.h config.h
$(encdir)\iso8859_2.obj:  $(encdir)\iso8859_2.c regenc.h config.h
$(encdir)\iso8859_3.obj:  $(encdir)\iso8859_3.c regenc.h config.h
$(encdir)\iso8859_4.obj:  $(encdir)\iso8859_4.c regenc.h config.h
$(encdir)\iso8859_5.obj:  $(encdir)\iso8859_5.c regenc.h config.h
$(encdir)\iso8859_6.obj:  $(encdir)\iso8859_6.c regenc.h config.h
$(encdir)\iso8859_7.obj:  $(encdir)\iso8859_7.c regenc.h config.h
$(encdir)\iso8859_8.obj:  $(encdir)\iso8859_8.c regenc.h config.h
$(encdir)\iso8859_9.obj:  $(encdir)\iso8859_9.c regenc.h config.h
$(encdir)\iso8859_10.obj: $(encdir)\iso8859_10.c regenc.h config.h
$(encdir)\iso8859_11.obj: $(encdir)\iso8859_11.c regenc.h config.h
$(encdir)\iso8859_13.obj: $(encdir)\iso8859_13.c regenc.h config.h
$(encdir)\iso8859_14.obj: $(encdir)\iso8859_14.c regenc.h config.h
$(encdir)\iso8859_15.obj: $(encdir)\iso8859_15.c regenc.h config.h
$(encdir)\iso8859_16.obj: $(encdir)\iso8859_16.c regenc.h config.h
$(encdir)\koi8.obj:       $(encdir)\koi8.c   regenc.h config.h
$(encdir)\koi8_r.obj:     $(encdir)\koi8_r.c regenc.h config.h
$(encdir)\cp1251.obj:     $(encdir)\cp1251.c regenc.h config.h
$(encdir)\big5.obj:       $(encdir)\big5.c   regenc.h config.h
$(encdir)\gb18030.obj:    $(encdir)\gb18030.c   regenc.h config.h

win32\onig.res: win32\onig.rc


# Ruby test
rtest:
	$(RUBYDIR)\win32\ruby -w -Ke test.rb

# C library test
ctest: $(testc)
	.\$(testc)

# POSIX C library test
ptest: $(testp)
	.\$(testp)

$(testc): $(testc).c $(libname)
	$(CC) -nologo -o $(testc) -DONIG_EXTERN=extern $(testc).c $(libname)

$(testp): $(testc).c $(dlllib)
	$(CC) -nologo -DPOSIX_TEST -o $(testp) $(testc).c $(dlllib)

#$(testc)u.c: test.rb testconvu.rb
#	ruby -Ke testconvu.rb test.rb > $@

$(testcu): $(testcu).c $(libname)
	$(CC) -nologo -o $(testcu) -DONIG_EXTERN=extern $(testcu).c $(libname)

$(libbase).def: win32\makedef.py oniguruma.h regenc.h oniggnu.h onigposix.h
	$(PYTHON) win32\makedef.py > $@

clean:
	-del *.obj $(encdir)\*.obj *.lib *.exp *.dll $(testp).exe $(testc).exe $(testcu).exe win32\*.res


# backup file suffix
SORIG = ruby_orig

# ruby 1.9 source update
19:
	$(CP) regerror.c    $(RUBYDIR)
	$(CP) regparse.c    $(RUBYDIR)
	$(CP) regcomp.c     $(RUBYDIR)
	$(CP) regexec.c     $(RUBYDIR)
	$(CP) regenc.c      $(RUBYDIR)
	$(CP) regint.h      $(RUBYDIR)
	$(CP) regparse.h    $(RUBYDIR)
	$(CP) regenc.h      $(RUBYDIR)
	$(CP) oniguruma.h   $(RUBYDIR)
	$(CP) enc\ascii.c   $(RUBYDIR)
	$(CP) enc\utf8.c    $(RUBYDIR)
	$(CP) enc\euc_jp.c  $(RUBYDIR)
	$(CP) enc\sjis.c    $(RUBYDIR)
	$(CP) enc\unicode.c $(RUBYDIR)


samples: all
	$(CC) $(CFLAGS) -I. -o simple  sample\simple.c  $(dlllib)
	$(CC) $(CFLAGS) -I. -o posix   sample\posix.c   $(dlllib)
	$(CC) $(CFLAGS) -I. -o names   sample\names.c   $(dlllib)
	$(CC) $(CFLAGS) -I. -o listcap sample\listcap.c $(dlllib)
	$(CC) $(CFLAGS) -I. -o sql     sample\sql.c     $(dlllib)
	$(CC) $(CFLAGS) -I. -o encode  sample\encode.c  $(dlllib)
	$(CC) $(CFLAGS) -I. -o syntax  sample\syntax.c  $(dlllib)
	$(CC) $(CFLAGS) -I. -o crnl    sample\crnl.c    $(dlllib)
