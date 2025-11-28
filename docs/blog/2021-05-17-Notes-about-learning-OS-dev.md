---
date: 2021-05-17
title: Learning OS dev notes
author: LMJW
tags: [OS, kernel]
---

# Notes about learning OS dev: [1] A minimal kernel

Below are list of references that I used to learn

- [A minimal Multiboot Kernel](https://os.phil-opp.com/multiboot-kernel)
- [HelloOS](https://gitee.com/lmos/cosmos/tree/master/lesson02/HelloOS)

## What is the minimal setting/configuration we need to have so we can boot a minimal kernel?

Assuming we are using GRUB boot loader, it seems to me we need to have 4 files to be able to boot a minimal kernel.

```bash
…
├── Makefile
└── src
    └── arch
        └── x86_64
            ├── multiboot_header.asm
            ├── boot.asm
            ├── linker.ld
            └── grub.cfg
```

Let's go through these files one by one.

### `multiboot_header.asm`

To know what is this, we need to know how boot works. (assuming we use BIOS firmware standard)

```
  ┌────────┐       ┌─────────┐       ┌────────────────────────────┐        ┌───────────┐
  │        │       │         │       │                            │        │           │
  │Power On├──────►│Load BIOS├──────►│Switch control to Bootloader├───────►│Load kernel│
  │        │       │         │       │                            │        │           │
  └────────┘       └─────────┘       └────────────────────────────┘        └───────────┘
```

In general, we will be using an existing bootloader, such as GRUB (which uses multiboot specification). In order to tell the bootloader our kernel will support the multiboot specification, we will need to have a multiboot header to let the bootloader that we are supporting multiboot.

```assembly
section .multiboot_header
header_start:
    dd 0xe85250d6                ; magic number (multiboot 2)
    dd 0                         ; architecture 0 (protected mode i386)
    dd header_end - header_start ; header length
    ; checksum
    dd 0x100000000 - (0xe85250d6 + 0 + (header_end - header_start)) ; small hack to avoid compiler warning

    ; insert optional multiboot tags here

    ; required end tag
    dw 0    ; type
    dw 0    ; flags
    dd 8    ; size
header_end: ; end of the file
```

### `boot.asm`

Now the bootloader understands that we are using multiboot, what's the next?

We need to add some code that bootloader can call. The way we do it is using `boot.asm`

```assembly
global start  ; export a lable

section .text ; default section for executing code
bits 32       ; specify the following lines are 32 bit execution
start:
    ; print `OK` to screen
    mov dword [0xb8000], 0x2f4b2f4f
    hlt ; tell CPU to stop
```
This basically defines a "function" but in assembly, it "prints" "OK" to screen and halt the CPU.

### `linker.ld`

Although the previous two steps, we have some assembly code, but we just prepare some seperate bits for different purposes, but they are not working together yet. We will combine them into an `ELF` executable so it can be run by the GRUB bootloader. The way we do this is through a `linker.ld` file.

```
ENTRY(start) /*entry point of the kernel*/

SECTIONS {
    . = 1M; /*conventional load address*/

    .boot :
    {
        /* ensure that the multiboot header is at the beginning */
        *(.multiboot_header)
    }

    .text :
    {
        *(.text)
    }
}
```

The commands we can run to get the ELF executable.

```
> nasm -f elf64 multiboot_header.asm
> nasm -f elf64 boot.asm
> ld -n -o kernel.bin -T linker.ld multiboot_header.o boot.o
```
**Note**: It's important to pass the -n (or --nmagic) flag to the linker, which disables the automatic section alignment in the executable. Otherwise the linker may page align the .boot section in the executable file. If that happens, GRUB isn't able to find the Multiboot header because it isn't at the beginning anymore.

After this step, we have an ELF executable called `kernel.bin`. But how can we `run` this kernel? This would require our to go to next step.

### `grub.cfg`

The `grub.cfg` is a config file of GRUB. And this file can also be used to create ISO. It is very simple to create an ISO.

#### Create a new ISO approach

1. create a folder structure like this

```
isofiles
└── boot
    ├── grub
    │   └── grub.cfg
    └── kernel.bin
```

2. What's in the `grub.cfg`?

```
set timeout=0
set default=0

menuentry "my os" {
    multiboot2 /boot/kernel.bin
    boot
}
```

3. run command `grub-mkrescue -o os.iso isofiles`. We will get an `os.iso` to be used for boot.

For example, we can boot our OS using `qemu-system-x86_64 -cdrom os.iso`

---

#### Using host OS to boot

However, we can also load the ELF file on exist OS without creating an ISO file. To add the option to boot from our new OS, we can add the following text in `/boot/grub/grub.cfg`

```
menuentry "my os" {
    insmod part_msdos
    insmod ext2
    set root='hd0,msdos4'         # `df /boot/` -> `/dev/sda4` -> `hd0,msdos4`
    multiboot2 /boot/kernel.bin
    boot
}
```

Then we copy our `kernel.bin` into `/boot/`. If we reboot. This should work as well.


## Conclusion

To summarize, we will mainly need 4 parts to be able to boot from a minimal kernel.

1. we need to tell bootloader GRUB what specification we are using (eg. mutiboot specification) -> `multiboot.asm`
2. we need to have a kernel program that can be run -> `boot.asm`
3. we need to create a ELF executable (combine header + kernel program) -> `kernel.bin`
4. we need to have a config to tell GRUB how to install/run the OS we created -> `grub.cfg`