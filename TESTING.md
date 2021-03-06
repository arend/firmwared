test-runner
===========

You can run intergration test via the test-runner. It will start a kernel and
then setups virtual machine via qemu. Finally it will run itself as pid 1.

It executes either all test it finds or you can provide a specific test
suite such as the firmware_tester.

Software Prerequisites
======================

The test-runner tool requires the following binaries to be present on the host
OS:

        Name:                   Tested ver.:
         1. qemu                2.4.1
         2. Linux kernel        https://git.kernel.org/pub/scm/linux/kernel/git/martineau/linux.git


Building Kernel
===============

The test-runner tool requires a kernel that is at least build with these
minimal options for a successful boot and execution:

        <arch>_defconfig                        Default kernel configuration

        kvmconfig                               Default configuration for
                                                kvm guests

        <firmwared>/tools/test_runner_kernel_config   The test-runner specific
                                                configuration

These configurations should be installed as .config in the kernel source
directory. To make a x86_64 guest kernel the sequence of commands may look
as follows:

        $ cd linux-X.X.X

        $ make x86_64_defconfig

        $ make kvmconfig

        $ sh <firmwared>/tools/test_runner_kernel_config

After that a default kernel with the required options can be built:

        $ make -j$(nproc)

By default the test-runner will search for the kernel image in these locations:

        <firmwared>/tools/bzImage

                or

        <firmwared>/tools/arch/x86/boot/bzImage

An arbitrary kernel image location can be specified by using '--kernel <path>'
parameter into test-runner.

Running Automated Tests
=======================

One can specify a particular set of test suite to be executed by using
providing the name of binary.

The command line may look as follows:

        $ ./test-runner firmware_tester

Or if a different kernel shall be tested and additinional debug logs
should be printed:

        $ ./test-runner -k ~/src/linux/arch/x86_64/boot/bzImage firmware_tester  -- -d

In order to run all tester the '-a' parameter can be used. Note with
autorun the 'quiet' option will also be added (-q):

        $ ./test-runner -a


firmware_tester
===============

The firmware_tester containts the real tests. You can run them either
using the test-runner or alternative you can run them on your target machine.

Let's assume your target machine is called rawhide.

        $ scp firmware_tester rawhide:
        $ scp firmwared rawhide:
        $ ssh rawhide ./firmware_tester

Note the firmware_tester has a dependency on glib and gio.
