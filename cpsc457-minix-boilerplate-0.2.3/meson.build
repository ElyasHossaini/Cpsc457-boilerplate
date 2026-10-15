# @file meson.build
#
# @author meevs
# @date 2026-08-28

project (
  'cpsc457-minix-boilerplate',
  version: '0.2.4',
  meson_version: '>=1.11.0'
)

sudo = find_program (
  'sudo',
  required: true
)
cp = find_program (
  'cp',
  required: true
)

mount_minix = subproject ('minix_from_scratch').get_variable ('mount_minix')
mount_minix_target = custom_target (
  'mount_minix',
  command: mount_minix,
  console: true,
  output: 'mount_minix'
)

cp_src_files_cmd = [
  sudo,
  cp,
  '--recursive',
  meson.project_source_root () / 'src',
  meson.project_build_root () / 'minix3bookmnt' / 'usr'
]
cp_src_files_target = custom_target (
  'cp_src_files',
  command: cp_src_files_cmd,
  depends: mount_minix_target,
  console: true,
  output: 'cp_src_files'
)

umount_minix = subproject ('minix_from_scratch').get_variable ('umount_minix')
umount_minix_target = custom_target (
  'umount_minix',
  command: umount_minix,
  depends: cp_src_files_target,
  console: true,
  output: 'umount_minix'
)

start_minix = subproject ('minix_from_scratch').get_variable ('start_minix')
start_minix_deps = [
  umount_minix_target
]
start_minix_target = custom_target (
  'start_minix',
  command: start_minix,
  depends: start_minix_deps,
  console: true,
  output: 'start_minix'
)

# Link the mount script
link_mount_vm_cmd = [
  'ln',
  '--symbolic',
  mount_minix,
  meson.project_build_root () / 'mount_vm.sh'
]
run_command (
  link_mount_vm_cmd,
  capture: true,
  check: true
)

# Link the umount script
link_umount_vm_cmd = [
  'ln',
  '--symbolic',
  umount_minix,
  meson.project_build_root () / 'umount_vm.sh'
]
run_command (
  link_umount_vm_cmd,
  capture: true,
  check: true
)

# Link run script
link_run_vm_cmd = [
  'ln',
  '--symbolic',
  start_minix,
  meson.project_build_root () / 'run_vm.sh'
]
run_command (
  link_run_vm_cmd,
  capture: true,
  check: true
)

copy_src_files_dict = {
  'MOUNT_VM_CMD': mount_minix.full_path (),
  'UMOUNT_VM_CMD': umount_minix.full_path (),
  'SRC_FILES_DIR': meson.project_source_root () / 'src',
  'MOUNT_DIR': meson.project_build_root () / 'minix3bookmnt' / 'usr'
}
configure_file (
  input: 'scripts/copy_src_files.sh.in',
  output: 'copy_src_files.sh',
  configuration: copy_src_files_dict
)


