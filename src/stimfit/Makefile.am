pkglib_LTLIBRARIES = libstimfit.la

libstimfit_la_SOURCES = ./stf.cpp ./math/fit.cpp \
            ./math/levmar/lm.c ./math/levmar/Axb.c ./math/levmar/misc.c ./math/levmar/lmlec.c ./math/levmar/lmbc.c \
            ./math/funclib.cpp ./math/stfmath.cpp ./math/measure.cpp \
            ./gui/app.cpp ./gui/unopt.cpp ./gui/doc.cpp ./gui/stfcheckbox.cpp ./gui/copygrid.cpp ./gui/graph.cpp \
            ./gui/printout.cpp ./gui/parentframe.cpp ./gui/childframe.cpp ./gui/view.cpp ./gui/table.cpp ./gui/zoom.cpp \
            ./gui/dlgs/cursorsdlg.cpp ./gui/dlgs/eventdlg.cpp \
	    ./gui/dlgs/fitseldlg.cpp ./gui/dlgs/smalldlgs.cpp \
            ./gui/usrdlg/usrdlg.cpp

libstimfit_la_CPPFLAGS = 
libstimfit_la_CXXFLAGS = $(OPT_CXXFLAGS) $(WX_CXXFLAGS)
libstimfit_la_LDFLAGS = $(LIBPYTHON_LDFLAGS) $(LIBLAPACK_LDFLAGS) $(LIBSTF_LDFLAGS)
libstimfit_la_LIBADD = $(WX_LIBS) ../libstfio/libstfio.la -lfftw3

# the application source, library search path, and link libraries
if BUILD_PYTHON
    PYTHON_ADDINCLUDES = $(LIBPYTHON_INCLUDES)
else
    PYTHON_ADDINCLUDES = 
endif

INCLUDES = $(PYTHON_ADDINCLUDES)
