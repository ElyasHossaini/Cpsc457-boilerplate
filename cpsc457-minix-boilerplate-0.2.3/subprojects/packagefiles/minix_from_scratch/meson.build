
project (
  'minix_from_scratch',
  version: '0.0.3'
)

start_minix_script = configure_file (
  input: 'scripts' / 'qemuminix3.sh',
  output: 'qemuminix3-console.sh',
  copy: true
)

minix3_harddisk = subproject ('minix3-harddisk').get_variable ('minix3_harddisk')

qemu = find_program (
  'qemu-i386',
  required: true
)

mount_minix = find_program (
  'mountminix3.sh',
  dirs: meson.project_source_root (),
  required: true,
)
meson.override_find_program (
  'mount_minix',
  mount_minix
)

umount_minix = find_program (
  'umountminix3.sh',
  dirs: meson.project_source_root (),
  required: true
)
meson.override_find_program (
  'umount_minix',
  umount_minix
)

start_minix = find_program (
  start_minix_script,
  dirs: meson.project_build_root () / 'subprojects' / 'minix_from_scratch',
  required: true
)
meson.override_find_program (
  'start_minix',
  start_minix
)

link_harddisk_cmd = [
  'ln',
  '--symbolic',
  minix3_harddisk,
  meson.global_build_root ()
]
run_command (
  link_harddisk_cmd,
  check: false
)
