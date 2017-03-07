import os


# Set required libraries
cppPaths            = ['.', 'include', '/usr/include/gdal', 
                       '/usr/include/gtest', 'usr/local/include']
cppDefines          = {}
libraries           = ['libgdal', 'gtest', 'pthread']
library_paths       =  
commonFlags         = ['-std=c++11', '-Wall']
optFlags            = ['-O2']
debugFlags          = ['-g']
profFlags           = ['-pg']
testFlags           = ['-pg',
                       '-Wstrict-overflow',
                       '-ftrapv',
                       '-fsanitize=address',
                       '-fsanitize=signed-integer-overflow',
                       '-fsanitize=null',
                       '-fsanitize-recover',
                       '-Wsign-compare',
                       '-Wsign-conversion',
                       '-fno-delete-null-pointer-checks',
                       '-fcatch-undefined-behavior]
releaseFlags        = ['-O2', '-fwrapv']

vars = Variables(None, ARGUMENTS)
vars.Add(EnumVariable('BUILD_TYPE', 'type of build to use', 'debug',  allowed_values=('debug', 'release', 'optimized')))

env = Environment(variables=vars)

if env['BUILD_TYPE'] == 'debug':
    print '*** debug build'

if env['BUILD_TYPE'] == 'release':
    print '*** release build'

if env['BUILD_TYPE'] == 'optimized':
    print '*** optimized build'

AddOption('--debug',
          dest='debug',
          type='string',
          nargs='1',
          action='store',
          help='debug build',
          default=False)

AddOption('--profile',
          dest='prof',
          type='string',
          nargs='1',
          action='store',
          help='profiling build',
          default=False)

AddOption('--test',
          dest='opt',
          type='string',
          nargs='1',
          action='store',
          help='optimization build',
          default=False)

AddOption('--release',
          dest='opt',
          type='string',
          nargs='1',
          action='store',
          help='optimization build',
          default=False)

# Set common environment variables
env = Environment(DEBUG = GetOption('debug'), OPT = GetOption('opt'),
                  PROF = GetOption('prof'))

# Required for Clang Static Analyzer
env["CC"] = os.getenv("CC") or env["CC"]
env["CXX"] = os.getenv("CXX") or env["CXX"]
env["ENV"].update(x for x in os.environ.items() if x[0].startswith("CCC_"))

# 
env.Append(OPT = 'opt')
env.Append(CPPPATH      = cppPaths)
env.Append(CPPDEFINES   = cppDefines)
env.Append(LIBS         = libraries)

# Check build flag, set CXXFLAGS
if (GetOption('debug')):
    env.Append(CXXFLAGS = debugFlags)
elif (GetOption('opt')):
    env.Append(CXXFLAGS = optFlags)
elif (GetOption('prof')):
    env.Append(CXXFLAGS = profFlags)
else:
    env.Append(CXXFLAGS = commonFlags)

# Alias all object targets (from /src and /test)
hello_o = Alias(env.Object(
                target = 'build/hello.o',
                source = 'src/hello.cpp'))

Alias("objs", [hello_o])

VariantDir('build-variant1', 'src')
SConscript('build-variant1/SConscript')
VariantDir('build-variant2', 'src')
SConscript('build-variant2/SConscript')

env.Program()