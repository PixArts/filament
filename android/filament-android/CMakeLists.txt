cmake_minimum_required(VERSION 3.4.1)

set(FILAMENT_DIR ${FILAMENT_DIST_DIR})

add_library(filament STATIC IMPORTED)
set_target_properties(filament PROPERTIES IMPORTED_LOCATION
        ${FILAMENT_DIR}/lib/${ANDROID_ABI}/libfilament.a)

add_library(utils STATIC IMPORTED)
set_target_properties(utils PROPERTIES IMPORTED_LOCATION
        ${FILAMENT_DIR}/lib/${ANDROID_ABI}/libutils.a)

add_library(filaflat STATIC IMPORTED)
set_target_properties(filaflat PROPERTIES IMPORTED_LOCATION
        ${FILAMENT_DIR}/lib/${ANDROID_ABI}/libfilaflat.a)

add_library(filabridge STATIC IMPORTED)
set_target_properties(filabridge PROPERTIES IMPORTED_LOCATION
        ${FILAMENT_DIR}/lib/${ANDROID_ABI}/libfilabridge.a)

add_library(bluevk STATIC IMPORTED)
set_target_properties(bluevk PROPERTIES IMPORTED_LOCATION
        ${FILAMENT_DIR}/lib/${ANDROID_ABI}/libbluevk.a)

include_directories(${FILAMENT_DIR}/include)

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -fno-exceptions -fno-rtti")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -ffast-math -ffp-contract=fast")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -fvisibility-inlines-hidden")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -fvisibility=hidden")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -ffunction-sections -fdata-sections")

set(CMAKE_SHARED_LINKER_FLAGS" ${CMAKE_SHARED_LINKER_FLAGS} -Wl,--gc-sections")
set(CMAKE_SHARED_LINKER_FLAGS" ${CMAKE_SHARED_LINKER_FLAGS} -Wl,-Bsymbolic-functions")
set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} -Wl,--version-script=${CMAKE_SOURCE_DIR}/libfilament-jni.map")

add_library(filament-jni SHARED
      src/main/cpp/Camera.cpp
      src/main/cpp/Colors.cpp
      src/main/cpp/VertexBuffer.cpp
      src/main/cpp/Engine.cpp
      src/main/cpp/EntityManager.cpp
      src/main/cpp/Fence.cpp
      src/main/cpp/IndexBuffer.cpp
      src/main/cpp/IndirectLight.cpp
      src/main/cpp/LightManager.cpp
      src/main/cpp/Material.cpp
      src/main/cpp/MaterialInstance.cpp
      src/main/cpp/MathUtils.cpp
      src/main/cpp/RenderableManager.cpp
      src/main/cpp/Renderer.cpp
      src/main/cpp/Scene.cpp
      src/main/cpp/SkyBox.cpp
      src/main/cpp/Stream.cpp
      src/main/cpp/Texture.cpp
      src/main/cpp/TextureSampler.cpp
      src/main/cpp/TransformManager.cpp
      src/main/cpp/View.cpp
      # Android specific
      src/main/cpp/nativewindow/Android.cpp
      # Private utils
      src/main/cpp/CallbackUtils.cpp
      src/main/cpp/Filament.cpp
      src/main/cpp/NioUtils.cpp
)

target_link_libraries(filament-jni
      filament
      filaflat
      filabridge
      utils
      log
      GLESv3
      EGL
      android
)

option(FILAMENT_SUPPORTS_VULKAN "Enables Vulkan on Android" OFF)

if (FILAMENT_SUPPORTS_VULKAN)
    target_link_libraries(filament-jni bluevk)
endif()
