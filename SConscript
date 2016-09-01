Import('env')

# Build the mbed library as a static library
# We really don't care about errors in the mbed library
env = env.Clone()
env.Append(CCFLAGS = ['-w'])

mbed_lib = env.MbedLikeLibrary(
  'mbed', 'mbed/hal/',
  ['api/', 'common/', 'hal/', 'targets/cmsis/', 'targets/hal/'],
)
env.Prepend(LIBS = mbed_lib)

env.Append(LINKFLAGS=[
  '-Wl,--whole-archive',  # used to compile mbed HAL, which uses funky weak symbols
  mbed_lib,
  '-Wl,--no-whole-archive',
  '--specs=nosys.specs',
])

# Build the user program with strict error checking
env = env.Clone()
env.Append(CCFLAGS = ['-Werror', '-Wall'])

program = env.Program(target = 'program', source = Glob('src/*.cpp'))
env.Depends(program, env['MBED_LINKSCRIPT'])

# Create a binary target for drag n' drop flashing
binary = env.Objcopy(program)

# Generate debugging information
env.Objdump(program)
env.SymbolsSize(program)
