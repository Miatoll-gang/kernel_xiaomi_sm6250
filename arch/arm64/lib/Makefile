# SPDX-License-Identifier: GPL-2.0
lib-y		:= bitops.o clear_user.o delay.o copy_from_user.o	\
		   copy_to_user.o copy_in_user.o copy_page.o		\
		   clear_page.o csum.o memchr.o memcpy.o memmove.o	\
		   memset.o memcmp.o strcmp.o strncmp.o strlen.o	\
		   strnlen.o strchr.o strrchr.o tishift.o

ifeq ($(CONFIG_KERNEL_MODE_NEON), y)
obj-$(CONFIG_XOR_BLOCKS)	+= xor-neon.o
CFLAGS_REMOVE_xor-neon.o	+= -mgeneral-regs-only
CFLAGS_xor-neon.o		+= -ffreestanding
endif

ifeq ($(CONFIG_KERNEL_MODE_NEON), y)
obj-$(CONFIG_XOR_BLOCKS)	+= xor-neon.o
CFLAGS_REMOVE_xor-neon.o	+= -mgeneral-regs-only
CFLAGS_xor-neon.o		+= -ffreestanding

# Tell the compiler to treat all general purpose registers (with the
# exception of the IP registers, which are already handled by the caller
# in case of a PLT) as callee-saved, which allows for efficient runtime
# patching of the bl instruction in the caller with an atomic instruction
# when supported by the CPU. Result and argument registers are handled
# correctly, based on the function prototype.
ifeq ($(ld-name),lld)
# https://bugs.llvm.org/show_bug.cgi?id=35841
obj-$(CONFIG_ARM64_LSE_ATOMICS) += atomic_ll_sc.o
else
lib-$(CONFIG_ARM64_LSE_ATOMICS) += atomic_ll_sc.o
endif

lib-$(CONFIG_ARCH_HAS_UACCESS_FLUSHCACHE) += uaccess_flushcache.o
