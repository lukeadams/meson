project('external library', 'c')

zlib = find_library('z')

# Verify that link testing works.
linkcode = '''#include<zlib.h>
int main(int argc, char **argv) {
  void *ptr = (void*)(deflate);
  return ptr == 0;
}
'''

nolinkcode='''int nonexisting();
int main(int argc, char **argv) {
  void *ptr = (void*)(nonexisting);
  return ptr == 0;
}
'''
cc = meson.get_compiler('c')
assert(cc.links(linkcode, args : '-lz', name : 'Test link against zlib'), 'Linking test failed.')
assert(not cc.links(nolinkcode, name : 'Failing link'), 'Linking succeeded when it should have failed.')

e = executable('zprog', 'prog.c', dependencies : zlib)
test('libtest', e)

# Test that ext deps work via an internal dep.
intdep = declare_dependency(dependencies : zlib)
exe2 = executable('zprog2', 'prog.c', dependencies : intdep)
test('libtest2', exe2)

# Test that deps that use find_library deps work.
depdep = declare_dependency(dependencies : intdep)
exe3 = executable('zprog3', 'prog.c', dependencies : depdep)
test('libtest3', exe3)
