set(PXR_PREFIX pxr/imaging)
set(PXR_PACKAGE hdx)

# XXX We tmp check for PXR_ENABLE_GL_SUPPORT since Hdx currently still uses
# OpenGL directly. Once Hdx uses only Hgi, remove GL_SUPPORT check.
if (NOT ${PXR_BUILD_GPU_SUPPORT} OR NOT ${PXR_ENABLE_GL_SUPPORT})
    message(STATUS
        "Skipping ${PXR_PACKAGE} because PXR_BUILD_GPU_SUPPORT is OFF")
    return()
endif()

set (optionalCppFiles "")
if (PXR_BUILD_OPENCOLORIO_PLUGIN)
    list(APPEND optionalLibs ${OCIO_LIBRARIES})
    list(APPEND optionalIncludeDirs ${OCIO_INCLUDE_DIRS})
endif()

pxr_library(hdx
    LIBRARIES
        plug
        tf
        vt
        gf
        work
        garch
        glf
        pxOsd
        hd
        hdSt
        hgi
        hgiInterop
        cameraUtil
        sdf
        ${optionalLibs}

    INCLUDE_DIRS
        ${optionalIncludeDirs}

    PUBLIC_CLASSES
        aovInputTask
        colorizeSelectionTask
        colorChannelTask
        colorCorrectionTask
        drawTargetTask
        freeCameraSceneDelegate
        fullscreenShader
        hgiConversions
        oitBufferAccessor
        oitRenderTask
        oitResolveTask
        oitVolumeRenderTask
        package
        pickTask
        pickFromRenderBufferTask
        presentTask
        renderTask
        renderSetupTask
        selectionTask
        selectionTracker
        shadowTask
        simpleLightTask
        skydomeTask
        task
        taskController
        tokens
        types
        visualizeAovTask

    PUBLIC_HEADERS
        shadowMatrixComputation.h
        version.h
        api.h

    PRIVATE_CLASSES
        debugCodes
        unitTestDelegate

    PRIVATE_HEADERS
        unitTestUtils.h

    RESOURCE_FILES
        plugInfo.json
        shaders/colorChannel.glslfx
        shaders/colorCorrection.glslfx
        shaders/fullscreen.glslfx
        shaders/oitResolveImageShader.glslfx
        shaders/outline.glslfx
        shaders/renderPass.glslfx
        shaders/renderPassColorShader.glslfx
        shaders/renderPassColorAndSelectionShader.glslfx
        shaders/renderPassColorWithOccludedSelectionShader.glslfx
        shaders/renderPassIdShader.glslfx
        shaders/renderPassPickingShader.glslfx
        shaders/renderPassOitShader.glslfx
        shaders/renderPassOitOpaqueShader.glslfx
        shaders/renderPassOitVolumeShader.glslfx
        shaders/renderPassShadowShader.glslfx
        shaders/selection.glslfx
        shaders/skydome.glslfx
        shaders/visualize.glslfx
        textures/StinsonBeach.hdr
        textures/StinsonBeach.tex

    DOXYGEN_FILES
        overview.dox
)

if (X11_FOUND OR APPLE)
pxr_build_test(testHdxRenderTask
    LIBRARIES
        hdx
        hd
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        testenv/testHdxRenderTask.cpp
)
pxr_build_test(testHdxLightAndShadow
    LIBRARIES
        hdx
        hd
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        testenv/testHdxLightAndShadow.cpp
)
endif()

if (X11_FOUND)
pxr_build_test(testHdxIdRender
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        testenv/testHdxIdRender.cpp
)
pxr_build_test(testHdxPickAndHighlight
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        unitTestUtils.cpp
        testenv/testHdxPickAndHighlight.cpp
)
pxr_build_test(testHdxCameraAndLight
    LIBRARIES
        hdx
        hd
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        testenv/testHdxCameraAndLight.cpp
)
pxr_build_test(testHdxDrawTarget
    LIBRARIES
        hdx
        hd
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        testenv/testHdxDrawTarget.cpp
)
pxr_build_test(testHdxPickingWithReprAndRefineChanges
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        unitTestUtils.cpp
        testenv/testHdxPickingWithReprAndRefineChanges.cpp
)
pxr_build_test(testHdxUnpickablesAsOccluders
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        unitTestUtils.cpp
        testenv/testHdxUnpickablesAsOccluders.cpp
)
pxr_build_test(testHdxPickTarget
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        unitTestUtils.cpp
        testenv/testHdxPickTarget.cpp
)
pxr_build_test(testHdxPickResolveMode
    LIBRARIES
        hdx
        hd
        hdSt
        sdf
        glf
        garch
        arch
        tf
    CPPFILES
        unitTestUtils.cpp
        testenv/testHdxPickResolveMode.cpp
)
endif()
