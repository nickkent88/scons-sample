# Based on https://github.com/dblalock/scons-example
import os


# Set required libraries
cppPaths            = ['.', 'include', 'usr/local/include']
cppDefines          = {}
cppFlags            = ['-Wall',
                       '-Wstrict-overflow']
cxxFlags            = ['-std=c++11']
libs                = ['libgdal', 'gtest', 'pthread']
libPaths            = ['/usr/include/gtest', '/usr/include/gdal']
testFlags           = ['-fcatch-undefined-behavior',
                       '-fno-delete-null-pointer-checks',
                       '-fsanitize=address',
                       '-fsanitize=null',
                       '-fsanitize-recover',
                       '-fsanitize=signed-integer-overflow',
                       '-ftrapv',
                       '-Wsign-compare',
                       '-Wsign-conversion',
                       '-pg']
releaseFlags        = ['-O2', '-fwrapv']


# define the attributes of the build environment shared between
# both the debug and release builds
common_env = Environment()
common_env.Append(LIBS 			= libraries)
common_env.Append(LIBPATH 		= libPaths)
common_env.Append(CPPPATH       = cppPaths)
common_env.Append(CPPDEFINES 	= cppDefines)
common_env.Append(CPPFLAGS 		= cppFlags)
common_env.Append(CXXFLAGS 		= cxxFlags)


# Required for Clang Static Analyzer
common_env["CC"] = os.getenv("CC") or common_env["CC"]
common_env["CXX"] = os.getenv("CXX") or common_env["CXX"]
common_env["ENV"].update(x for x in os.environ.items() if x[0].startswith("CCC_"))


# Our release build is derived from the common build environment...
release_env = common_env.Clone()
release_env.Append(CPPDEFINES=['RELEASE'])
release_env.VariantDir('build/release/src', 'src', duplicate=0)
release_env.VariantDir('build/release/test', 'test', duplicate=0)


# We define our debug build environment in a similar fashion...
debug_env = common_env.Clone()
debug_env.Append(CPPDEFINES=['DEBUG'])
debug_env.VariantDir('build/debug/src', 'src', duplicate=0)
debug_env.VariantDir('build/debug/test', 'test', duplicate=0)


# Now that all build environment have been defined, let's iterate over
# them and invoke the lower level SConscript files.
for mode, env in dict(release=release_env, debug=debug_env).items():
    modeDir = 'build/%s' % mode
    env.SConscript('%s/src/sconscript' % modeDir, {'env': env})
    env.SConscript('%s/test/sconscript' % modeDir, {'env': env})
