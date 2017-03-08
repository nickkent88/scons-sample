# Based on https://github.com/dblalock/scons-example
import os


# Set required libraries
cppPaths            = ['.',
                       '../../..',
                       'usr/local/include',
                       '/usr/include/gtest',
                       '/usr/include/gdal']
cppDefines          = {}
cppFlags            = ['-Wall',
                       '-Wcast-align',
                       '-Wdiv-by-zero',
                       '-Wextra',
                       '-Wfloat-equal',
                       '-Wframe-address',
                       '-Wlogical-op',
                       '-Wmissing-declarations',
                       '-Wnull-dereference',
                       '-Woverloaded-virtual',
                       '-Wpointer-arith',
                       '-Wredundant-decls',
                       '-Wshadow',
                       '-Wsign-conversion',
                       '-Wsuggest-attribute=const',
                       '-Wsuggest-attribute=noreturn',
                       '-Wsuggest-attribute=pure',
                       '-Wswitch',
                       '-Wswitch-bool',
                       '-Wswitch-default',
                       '-Wswitch-enum',
                       '-Wundef',
                       '-Wuninitialized',
                       '-Wvector-operation-performance',
                       '-Wwrite-strings',
                       '-pedantic']
cxxFlags            = ['-std=c++11']
libs                = ['libgdal', 'gtest', 'pthread']
libPaths            = []
debugFlags          = ['-fsanitize=float-cast-overflow',
                       '-fstack-check',
                       '-fsanitize=leak',
                       '-fsanitize=shift',
                       '-fsanitize=return',
                       '-fsanitize=bounds-strict',
                       '-fsanitize=float-divide-by-zero',
                       '-fsanitize=unreachable',
                    #    The following commented-out lines are broken in Mac OS Sierra
                    #    '-fsanitize=null',
                    #    '-fsanitize=undefined',
                    #    '-fsanitize=vptr',
                       '-fsanitize-recover', # gcc
                    #    '-fsanitize-recover=undefined', #clang
                    #    '-fsanitize-recover=integer', #clang
                       '-fsanitize=signed-integer-overflow',
                       '-ftrapv',
                       '-pg']
releaseFlags        = ['-O2', '-fwrapv', '-fno-delete-null-pointer-checks']

# define the attributes of the build environment shared between
# both the debug and release builds
common_env = Environment()
common_env.Append(LIBS 			= libs)
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
release_env.Append(CPPFLAGS=releaseFlags)

# We define our debug build environment in a similar fashion...
debug_env = common_env.Clone()
debug_env.Append(CPPDEFINES=['DEBUG'])
debug_env.VariantDir('build/debug/src', 'src', duplicate=0)
debug_env.VariantDir('build/debug/test', 'test', duplicate=0)
debug_env.Append(CPPFLAGS=debugFlags)

# Now that all build environment have been defined, let's iterate over
# them and invoke the lower level SConscript files.
for mode, env in (('release', release_env), ('debug', debug_env)):
    modeDir = 'build/%s' % mode
    env.SConscript('%s/src/sconscript' % modeDir, {'env': env})
    env.SConscript('%s/test/sconscript' % modeDir, {'env': env})
