va_sources = [
  'plugin.c',
  'gstjpegdecoder.c',
  'gstvabasedec.c',
  'gstvabasetransform.c',
  'gstvabaseenc.c',
  'gstvacaps.c',
  'gstvacompositor.c',
  'gstvadecoder.c',
  'gstvadeinterlace.c',
  'gstvadevice.c',
  'gstvadisplay_priv.c',
  'gstvaencoder.c',
  'gstvafilter.c',
  'gstvah264dec.c',
  'gstvah264enc.c',
  'gstvah265dec.c',
  'gstvah265enc.c',
  'gstvajpegdec.c',
  'gstvampeg2dec.c',
  'gstvaprofile.c',
  'gstvavp8dec.c',
  'gstvavp9dec.c',
  'gstvavpp.c',
]

if host_system != 'linux'
  subdir_done()
endif

va_option = get_option('va').require(gstva_dep.found(), error_message: 'va plugin requires libgstva.')
if va_option.disabled()
  subdir_done()
endif

libgudev_dep = dependency('gudev-1.0', required: false)
cdata.set10('HAVE_GUDEV', libgudev_dep.found())

if libva_dep.version().version_compare('>= 1.8')
  va_sources += 'gstvaav1dec.c'
endif

driverdir = libva_dep.get_variable('driverdir', default_value: '')
if driverdir == ''
  driverdir = join_paths(get_option('prefix'), get_option('libdir'), 'dri')
endif
gstva_cargs = [
  '-DLIBVA_DRIVERS_PATH="' + driverdir + '"',
  '-std=c99',
  '-DGST_USE_UNSTABLE_API',
]

gstva = library('gstva',
  va_sources,
  c_args : gst_plugins_bad_args + gstva_cargs,
  include_directories : [configinc],
  dependencies : [gstcodecs_dep, gstva_dep, libgudev_dep] + extra_dep,
  install : true,
  install_dir : plugins_install_dir,
)
plugins += [gstva]
