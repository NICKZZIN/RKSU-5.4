# --- FORÇA BRUTA DE CONFIGURAÇÕES RKSU + SUSFS ---
ccflags-y += -DCONFIG_KSU=1
ccflags-y += -DCONFIG_KSU_MANUAL_HOOK=1
ccflags-y += -DCONFIG_KSU_SUSFS=1
ccflags-y += -DCONFIG_KSU_SUSFS_SUS_MOUNT=1
ccflags-y += -DCONFIG_KSU_SUSFS_SUS_PATH=1
ccflags-y += -DCONFIG_KSU_SUSFS_SUS_KSTAT=1
ccflags-y += -DCONFIG_KSU_SUSFS_FORBID_OPEN_KSU_PATCH=1
# -------------------------------------------------
KDIR := $(KDIR)
MDIR := $(realpath $(dir $(abspath $(lastword $(MAKEFILE_LIST)))))

$(info -- KDIR: $(KDIR))
$(info -- MDIR: $(MDIR))

.PHONY: all compdb clean format check-format

all: check_symbol
	make -C $(KDIR) M=$(MDIR) modules
	./check_symbol kernelsu.ko $(KDIR)/vmlinux
compdb:
	python3 $(MDIR)/.vscode/generate_compdb.py -O $(KDIR) $(MDIR)
clean:
	make -C $(KDIR) M=$(MDIR) clean
	rm check_symbol
check_symbol: tools/check_symbol.c
	$(CC) tools/check_symbol.c -o check_symbol
format:
	find . \( -name "*.c" -o -name "*.h" \) -print0 | xargs -0 clang-format -i
check-format:
	find . \( -name "*.c" -o -name "*.h" \) -print0 | xargs -0 clang-format --dry-run --Werror

# Keep a new line here!! Because someone may append config
