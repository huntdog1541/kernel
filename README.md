SOS: the Stupid Operating System
--------------------------------

SOS is a simple, tiny toy OS implemented in Rust.

I'm writing this mostly for fun, to learn more about OS design and kernel hacking, so don't expect anything new or exciting out of this project.

Inspiration, and a reasonable amount of code, taken from @phil-opp's great [series of blog posts](http://os.phil-opp.com) on the subject, Charlie Somerville's [rustboot](https://github.com/charliesome/rustboot), and Samy Pessé's [_How to Make an Operating System_](https://www.gitbook.com/book/samypesse/how-to-create-an-operating-system/details).

### Design goals

 + **POSIX compliance is not a goal** (though it would be cool)
 + **Hybrid/loosely microkernel** (i.e., move code to user space *when convenient/practical*)
 + Possibly provide the Rust stdlib at the OS level.
 + JVM-style memory allocation?
 + Possibly experiment with a [Plan 9-esque](https://en.wikipedia.org/wiki/9P_(protocol)) networking stack eventually?


### Building & Running

This project includes Git submodules; you will want to clone it using `git clone --recursive` or run `git submodule update --init` after cloning.

#### You will need:

+ Rust; I strongly recommend installing it using [multirust](https://github.com/brson/multirust)
+ `nasm`
+ `grub-mkrescue` and possibly `xorriso` depending on whether your system considers it to be part of the `mkrescue` package (ugh)
+ `qemu`, specifically `qemu-system-x86_64`; unless you want to try SOS on bare metal (which I cannot possibly endorse)

Alternatively, if you want to use Vagrant to get a working development environment right out of the box, all you need is Vagrant installed. The Vagrantfile in this repo will take care of automatically provisioning a dev environment with everything you need to build the OS.

+ **Setting up**
    + You only need to run these steps once
    + `$ multirust override nightly-2015-11-08` this version of Rust is known to work with our patched version of `libcore`. This step is not necessary if you are using Vagrant, as the Vagrantfile already selects this Rust version when provisioning the VM.
    + `$ make runtime` compiles the patched `libcore`
+ **Running the OS**
    + `$ make run` compiles the kernel, makes the ISO, and boots QEMU from the ISO

#### Using Vagrant

To avoid tooling and dependency hell (especially on Macs), I'm using a Vagrant config written by @raphael-enochian based on one by @ashleygwilliams (see her repo [here](https://github.com/ashleygwilliams/x86-kernel)). Vagrant will ensure you have a dev environment with everything necessary to build SOS right out of the box.

To run using vagrant (from the repo root directory):

 + `$ vagrant up`
 + `$ vagrant ssh -- -Y`
 + `$ cd /vagrant`
 + Follow the instructions from above
