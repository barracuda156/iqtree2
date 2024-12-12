if(POWERARCH)
  set(BIONJ2_H "")
  set(BIONJ2_CPP "")
else()
  set(BIONJ2_H "bionj2.h")
  set(BIONJ2_CPP "bionj2.cpp")
endif()

add_library(utils
eigendecomposition.cpp eigendecomposition.h
gzstream.cpp gzstream.h
optimization.cpp optimization.h
stoprule.cpp stoprule.h
tools.cpp tools.h
pllnni.cpp pllnni.h
checkpoint.cpp checkpoint.h
MPIHelper.cpp MPIHelper.h
starttree.cpp starttree.h
bionj.cpp ${BIONJ2_CPP} ${BIONJ2_H}
progress.cpp progress.h
timeutil.h hammingdistance.h
operatingsystem.cpp operatingsystem.h
heapsort.h
)

if(ZLIB_FOUND)
  target_link_libraries(utils ${ZLIB_LIBRARIES})
else(ZLIB_FOUND)
  target_link_libraries(utils zlibstatic)
endif(ZLIB_FOUND)

target_link_libraries(utils lbfgsb sprng)

add_executable(decentTree
    decenttree.cpp
    starttree.cpp bionj.cpp ${BIONJ2_CPP}
    gzstream.cpp progress.cpp operatingsystem.cpp)

if(ZLIB_FOUND)
  target_link_libraries(decentTree ${ZLIB_LIBRARIES})
else(ZLIB_FOUND)
  target_link_libraries(decentTree zlibstatic)
endif(ZLIB_FOUND)

if(CLANG AND WIN32)
    target_link_libraries(decentTree ${PROJECT_SOURCE_DIR}/lib/libiomp5md.dll)
endif()
