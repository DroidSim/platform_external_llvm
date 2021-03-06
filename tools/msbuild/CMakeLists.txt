if (WIN32)
  # CPack will install a registry key in this format that we wish to reference.
  set(REG_KEY "${CPACK_PACKAGE_INSTALL_REGISTRY_KEY}")
  set(LIB_PATH_VERSION "${LLVM_VERSION_MAJOR}.${LLVM_VERSION_MINOR}")

  foreach (platform "Win32" "x64")
    set(prop_file_in "Microsoft.Cpp.Win32.llvm.props.in")
    set(prop_file_v100 "Microsoft.Cpp.${platform}.LLVM-vs2010.props")
    set(prop_file_v110 "Microsoft.Cpp.${platform}.LLVM-vs2012.props")
    set(prop_file_v110_xp "Microsoft.Cpp.${platform}.LLVM-vs2012_xp.props")
    set(prop_file_v120 "toolset-vs2013.props")
    set(prop_file_v120_xp "toolset-vs2013_xp.props")

    if (platform STREQUAL "Win32")
      set(mflag "m32")
    else()
      set(mflag "m64")
    endif()
    set(VS_VERSION "v100")
    set(MSC_VERSION "1600")
    configure_file(${prop_file_in} ${platform}/${prop_file_v100})
    set(VS_VERSION "v110")
    set(MSC_VERSION "1700")
    configure_file(${prop_file_in} ${platform}/${prop_file_v110})
    set(VS_VERSION "v110_xp")
    configure_file(${prop_file_in} ${platform}/${prop_file_v110_xp})
    set(VS_VERSION "v120")
    set(MSC_VERSION "1800")
    configure_file(${prop_file_in} ${platform}/${prop_file_v120})
    set(VS_VERSION "v120_xp")
    configure_file(${prop_file_in} ${platform}/${prop_file_v120_xp})
    set(VS_VERSION)
    set(MSC_VERSION)
    set(mflag)

    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${platform}/${prop_file_v100}" DESTINATION tools/msbuild/${platform})
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${platform}/${prop_file_v110}" DESTINATION tools/msbuild/${platform})
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${platform}/${prop_file_v110_xp}" DESTINATION tools/msbuild/${platform})
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${platform}/${prop_file_v120}" DESTINATION tools/msbuild/${platform})
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${platform}/${prop_file_v120_xp}" DESTINATION tools/msbuild/${platform})

    install(FILES "Microsoft.Cpp.Win32.LLVM-vs2010.targets" DESTINATION "tools/msbuild/${platform}" RENAME "Microsoft.Cpp.${platform}.LLVM-vs2010.targets")
    install(FILES "Microsoft.Cpp.Win32.LLVM-vs2012.targets" DESTINATION "tools/msbuild/${platform}" RENAME "Microsoft.Cpp.${platform}.LLVM-vs2012.targets")
    install(FILES "Microsoft.Cpp.Win32.LLVM-vs2012_xp.targets" DESTINATION "tools/msbuild/${platform}" RENAME "Microsoft.Cpp.${platform}.LLVM-vs2012_xp.targets")
    install(FILES "toolset-vs2013.targets" DESTINATION "tools/msbuild/${platform}")
    install(FILES "toolset-vs2013_xp.targets" DESTINATION "tools/msbuild/${platform}")
  endforeach()

  set(LIB_PATH_VERSION)
  set(REG_KEY)

  install(DIRECTORY .
    DESTINATION tools/msbuild
    FILES_MATCHING
    PATTERN "*.bat"
    PATTERN ".svn" EXCLUDE
    )
endif()
