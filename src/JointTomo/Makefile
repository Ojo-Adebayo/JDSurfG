out = JointTomo

# compiler and options
include ../../include/Makefile
cc = g++
FC = gfortran
FFLAGS  = -ffixed-line-length-none -ffloat-store\
           -w -m64 -mcmodel=medium
includes = -I../../include $(path_to_eigen) $(path_to_gsl_include)
gsl_lib = $(path_to_gsl_lib)
surfdir= ../SurfaceWave
utils = ../utils

# c++ scripts
src = JointTomo.cpp main.cpp $(surfdir)/SurfaceWave.cpp $(utils)/gaussian.cpp \
	   $(utils)/spherical.cpp $(utils)/gauleg.cpp ../gravity/gravmat.cpp \
	   $(utils)/bkg_model.cpp


# fortran src
f90mp=$(surfdir)/fmm.f90 $(surfdir)/sphfmm.f90 
f90src =  ../utils/delsph.f90 ../utils/empirical.f90 ../utils/lsmrModule_csr.f90
f77src = $(surfdir)/surfdisp96.f

# objective
cppobj = $(src:%.cpp=%.o)
fobj= $(f90src:%.f90=%.o) $(f90mp:%.f90=%.o) $(f77src:%.f=%.o) 

$(out): $(fobj) $(cppobj)
	$(cc)  -o ../../bin/$(out) $(fobj) $(cppobj) -lgfortran -fopenmp $(gsl_lib)

%.o: %.f90
	$(FC) -g -c $< -o $@  $(opt)

$(surfdir)/SurfaceWave.o: $(surfdir)/SurfaceWave.cpp
	$(cc) -g -c $(cppstd) $(includes)  $< -o $@ $(opt) -fopenmp

%.o: %.f
	$(FC) $(FFLAGS) -g -rdynamic -c $< -o $@ $(opt)	

%.o: %.cpp
	$(cc) -g -c $(cppstd) $(includes) $< -o $@ $(opt)

$(surfdir)/sphfmm.o: $(surfdir)/sphfmm.f90
	$(FC) -g -c $< -o $@  $(opt) -fopenmp

 $(surfdir)/fmm.o: $(surfdir)/fmm.f90
	$(FC) -g -c $< -o $@  $(opt) -fopenmp
clean:
	rm  *.o *.mod  $(utils)/*.o 
