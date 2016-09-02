# Top-level build wrapper so build outputs go in a separate directory.
import multiprocessing
import os

SetOption('num_jobs', multiprocessing.cpu_count() + 1)

# Force Linux-style parameters for C/C++ compiles, even under Windows (where it
# defaults to MSVC style parameters that GCC-ARG doesn't like)
env = Environment(ENV={'PATH' : os.environ['PATH']}, tools=['mingw'])
Export('env')

SConscript('mbed-scons/SConscript-mbed')

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
    'RANLIBCOMSTR': 'IND',
    'BINCOMSTR': 'BIN',
    'OBJDUMPCOMSTR': 'DUMP',
    'SYMBOLSCOMSTR': 'DUMP',
    'SYMBOLSIZESCOMSTR': 'DUMP',
  })

###
### Additional environment setup
###
SConscript('mbed-scons/SConscript-env-platforms')
SConscript('mbed-scons/SConscript-env-gcc-arm')

###
### Actual build targets here
###
SConscript('SConscript', variant_dir='build', duplicate=0)
