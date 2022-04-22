SRC_NAME = vim
THISDIR = $(shell pwd)
LDFLAGS += -L$(STAGEDIR)/lib -L$(STAGEDIR)/bin -lncurses

all: download_test config_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -d vim ]; then \
		git clone https://github.com/vim/vim.git
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd vim ; \
	ac_cv_header_elf_h=no \
	vim_cv_getcwd_broken=no \
	vim_cv_memmove_handles_overlap=yes \
	vim_cv_stat_ignores_slash=yes \
	vim_cv_tgetent=zero \
	vim_cv_terminfo=yes \
	vim_cv_toupper_broken=no \
	vim_cv_tty_group=root \
	vim_cv_tty_mode=0620 \
	./configure \
		--prefix=/usr \
		--with-features=tiny \
		--disable-gui \
		--disable-gtktest \
		--disable-xim \
		--without-x \
		--disable-netbeans \
		--disable-cscope \
		--disable-gpm \
		--disable-acl \
		--disable-selinux \
		--with-tlib=ncurses \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/vim /usr/bin/vim
