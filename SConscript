Import('env')

env.ConfigureMbedTarget('NUCLEO_L432KC', File('mbed/hal/targets.json').srcnode())
env.Append(CCFLAGS = '-Os')

# Build the mbed library as a static library
mbed_paths = env.GetMbedSourceDirectories('mbed/hal')
env.Append(CPPPATH=[x.srcnode() for x in mbed_paths])  # this allows duplicate=0
env['MBED_LINKSCRIPT'] = env.GetMbedLinkscript(mbed_paths)

env_mbed = env.Clone()
env_mbed.Append(CCFLAGS = ['-w'])  # don't care about errors in dependencies
mbed_lib = env_mbed.StaticLibrary('mbed', env.GetMbedSources(mbed_paths))

env.Prepend(LIBS = mbed_lib)
env.Append(LINKFLAGS=[
  '-Wl,--whole-archive',  # used to compile mbed HAL, which uses funky weak symbols
  mbed_lib,
  '-Wl,--no-whole-archive',
  '--specs=nosys.specs',
])

env.Append(CCFLAGS = ['-Werror', '-Wall'])  # strict warnings in user program

program = env.Program('program', Glob('src/*.cpp'))
binary = env.Binary(program)  # for drag n' drop flashing

# Generate debugging information
env.Objdump(program)
env.SymbolsSize(program)
