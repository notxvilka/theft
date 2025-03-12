project('theft', 'c',
  version : '0.4.6',
  default_options : [
    'c_std=c99',
    'warning_level=3',
    'optimization=3',
    'b_pgo=off',
    'b_coverage=false',
    'b_pie=true'
  ]
)

c_args = [
  '-D_POSIX_C_SOURCE=199309L',
  '-D_C99_SOURCE',
  '-pedantic'
]

inc_dirs = include_directories('inc', 'vendor')

theft_sources = [
  'src/theft.c',
  'src/theft_autoshrink.c',
  'src/theft_bloom.c',
  'src/theft_call.c',
  'src/theft_hash.c',
  'src/theft_random.c',
  'src/theft_rng.c',
  'src/theft_run.c',
  'src/theft_shrink.c',
  'src/theft_trial.c',
  'src/theft_aux.c',
  'src/theft_aux_builtin.c'
]

test_sources = [
  'test/test_theft.c',
  'test/test_theft_autoshrink.c',
  'test/test_theft_autoshrink_ll.c',
  'test/test_theft_autoshrink_bulk.c',
  'test/test_theft_autoshrink_int_array.c',
  'test/test_theft_aux.c',
  'test/test_theft_bloom.c',
  'test/test_theft_error.c',
  'test/test_theft_prng.c',
  'test/test_theft_integration.c',
  'test/test_char_array.c'
]

cc = meson.get_compiler('c')

# m-library dependency handling
mdep = cc.find_library('m', required: false)

# Generate bits_lut.h
bits_lut_gen = custom_target('bits_lut',
  output : 'bits_lut.h',
  command : ['scripts/mk_bits_lut'],
  capture : true
)

theft_lib = library('theft',
  theft_sources,
  dependencies : mdep,
  include_directories : inc_dirs,
  c_args : c_args,
  install : true
)

theft_dep = declare_dependency(
    link_with : theft_lib,
    dependencies : mdep,
    include_directories : include_directories('inc'),
)

test_exe = executable('test_theft',
  test_sources,
  link_with : theft_lib,
  include_directories : [inc_dirs, include_directories('src')],
  c_args : c_args,
  dependencies : mdep
)

install_headers('inc/theft.h', 'inc/theft_types.h')

pkg = import('pkgconfig')
pkg.generate(
  name : 'theft',
  description : 'Property-based testing for C',
  libraries : theft_lib
)

test('theft tests', test_exe)
