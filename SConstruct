# Top-level build wrapper so build outputs go in a separate directory.
import multiprocessing
import os

SetOption('num_jobs', multiprocessing.cpu_count() + 1)

# Force Linux-style parameters for C/C++ compiles, even under Windows (where it
# defaults to MSVC style parameters that GCC-ARG doesn't like)
env = Environment(ENV={'PATH' : os.environ['PATH']}, tools=['mingw'])
Export('env')

##
## Go fast!
##
env.Decider('MD5-timestamp')

##
## Debugging output optimization
##
def simplify_output(env, mappings):
  pad_len = max([len(val) for val in mappings.values()]) + 2
  for key, val in mappings.items():
    env[key] = val + (' ' * (pad_len - len(val))) + '$TARGET'

if ARGUMENTS.get('VERBOSE') != '1':
  simplify_output(env, {
    'ASPPCOMSTR': 'AS',
    'ASCOMSTR': 'AS',
    'ARCOMSTR': 'AR',
    'CCCOMSTR': 'CC',
    'CXXCOMSTR': 'CXX',
    'LINKCOMSTR': 'LD',
    'RANLIBCOMSTR': 'RANLIB',
    'OBJCOPYCOMSTR': 'BIN',
    'OBJDUMPCOMSTR': 'DUMP',
    'SYMBOLSCOMSTR': 'SYM',
    'SYMBOLSIZESCOMSTR': 'SYM',
  })

###
### Imports
###
SConscript('mbed-scons/SConscript-env-platforms', duplicate=0)
SConscript('mbed-scons/SConscript-env-gcc-arm', duplicate=0)
SConscript('mbed-scons/SConscript-mbed', duplicate=0)

###
### Platform-specific build targets for mbed libraries
###
env.Append(CCFLAGS = '-Os')
env['MBED_LIB_LINKSCRIPTS_ROOT'] = Dir('mbed/hal').srcnode()
env['MBED_TARGETS_JSON_FILE'] = File('mbed/hal/targets.json').srcnode()

SConscript('mbed-scons/targets/SConscript-mbed-env-stm32l432kc', duplicate=0)

###
### Actual build targets here
###
SConscript('SConscript', variant_dir='build', duplicate=0)
