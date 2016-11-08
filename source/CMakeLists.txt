# root_generate_dictionary(ostap_dict ${CMAKE_CURRENT_SOURCE_DIR}/dict/Dict.h ${CMAKE_CURRENT_SOURCE_DIR}/dict/selections.xml)
set(BOOSTROOT1 /afs/cern.ch/sw/lcg/releases/LCG_86/Boost/1.62.0/x86_64-slc6-gcc49-opt)
set(Boost_NO_SYSTEM_PATHS ON)
set(BOOST_INCLUDEDIR ${BOOSTROOT1}/include/boost-1_62)
set(BOOST_LIBRARYDIR ${BOOSTROOT1}/lib)
find_package(ROOT  REQUIRED COMPONENTS Smatrix Core MathCore MathMore Hist Matrix RIO Tree Thread TreePlayer RooFit RooFitCore PyROOT)
find_package(Boost REQUIRED)
find_package(GSL   REQUIRED)

function( MAKE_DICT name header selection )
   REFLEX_GENERATE_DICTIONARY( ${name} ${header} SELECTION ${selection} )
   add_library( ${name}Dict SHARED ${name}.cxx )
   add_dependencies(${name}Dict ${name}-dictgen)
   #set_target_properties( ${name} PROPERTIES COMPILE_FLAGS ${ROOT_CXX_FLAGS} )
   target_link_libraries( ${name}Dict ${ROOT_LIBRARIES} ostap ${ROOT_LIBRARIES} ostap ${ROOT_LIBRARIES} )
endfunction( MAKE_DICT )


#---Define useful ROOT functions and macros (e.g. ROOT_GENERATE_DICTIONARY)
include(${ROOT_USE_FILE})

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include ${Boost_INCLUDE_DIRS} ${GSL_INCLUDE_DIRS})
#REFLEX_GENERATE_DICTIONARY(ostap_dict src/dict/Dict.h SELECTION src/dict/selections.xml)
MAKE_DICT (ostap src/dict/Ostap.hh src/dict/Ostap.xml )

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/ostap_rdict.pcm   DESTINATION ${LIBRARY_OUTPUT_PATH})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/ostapDict.rootmap DESTINATION ${LIBRARY_OUTPUT_PATH})
#---Create a shared library with geneated dictionary
add_library(ostap SHARED src/BSpline.cpp
                         src/Bernstein.cpp
                         src/Binomial.cpp
                         src/Choose.cpp
                         src/Chi2Fit.cpp
                         src/EigenSystem.cpp   
                         src/Exception.cpp
                         src/Faddeeva.cpp 
                         src/Formula.cpp   
                         src/GSL_sentry.cpp 
                         src/GSL_utils.cpp 
                         src/Hesse.cpp
                         src/HistoDump.cpp
                         src/HistoInterpolation.cpp
                         src/HistoMake.cpp
                         src/HistoProject.cpp
                         src/HistoStat.cpp
                         src/Interpolation.cpp
                         src/Iterator.cpp
                         src/Lomont.cpp
                         src/LorentzVectorWithError.cpp
                         src/Math.cpp
                         src/Models.cpp
                         src/MoreMath.cpp
                         src/Mute.cpp
                         src/NStatEntity.cpp
                         src/PDFs.cpp
                         src/Point3DWithError.cpp
                         src/Polynomials.cpp
                         src/PyIterator.cpp
                         src/PySelector.cpp
                         src/PySelectorWithCuts.cpp
                         src/SFactor.cpp
                         src/StatEntity.cpp
                         src/StatVar.cpp
                         src/StatusCode.cpp
                         src/Tee.cpp
                         src/UStat.cpp
                         src/ValueWithError.cpp
                         src/Vector3DWithError.cpp
                         src/WStatEntity.cpp    
                         src/nSphere.cpp       )    

target_link_libraries(ostap ${ROOT_LIBRARIES} ${GSL_LIBRARIES})

# #---Create  a main program using the library
# add_executable(Main MainEvent.cxx)
# target_link_libraries(Main Event)