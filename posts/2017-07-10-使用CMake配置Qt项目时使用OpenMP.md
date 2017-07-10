---
title: 使用CMake配置Qt项目时使用OpenMP
date: '2017-07-10'
description:
categories:

tags:system

---

>

### OpenMP

>

	// CMakeLists.txt
	FIND_PACKAGE( OpenMP REQUIRED)
	if(OPENMP_FOUND)
	    message("OPENMP FOUND")
	    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
	    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
	    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
	endif()

>

	// 测试
	#include <omp.h>
	#pragma omp parallel
        {
		printf("hello world! ThreadID = %d\n", omp_get_thread_num());
        }
        cout << endl;

>

