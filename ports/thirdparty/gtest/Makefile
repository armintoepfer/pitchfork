include ../../../mk/config.mk

# Local variables
_NAME            = gtest-1.7.0
_URL             = https://github.com/google/googletest
_FILE            = release-1.7.0.tar.gz
_WRKSRC          = $(WORKSPACE)/$(_NAME)

# Local works
do-extract: wscheck
	@test -e $(_FILE) || $(CURL) -L -O $(_URL)/archive/$(_FILE)
	@which $(MD5SUM) >& /dev/null && $(MD5SUM) -c MD5SUM
	@test -d $(_WRKSRC) || tar zxf $(_FILE) --transform 's/^googletest-release/gtest/' -C $(WORKSPACE)
do-clean:
	@test -d $(_WRKSRC) && rm -rf $(_WRKSRC) || true
do-build: do-extract
do-install: do-build pfcheck $(PREFIX)/var/pkg/$(_NAME)
$(PREFIX)/var/pkg/$(_NAME):
	@find $(PREFIX) ! -type d > $(_WRKSRC).before.list
	@cd $(_WRKSRC) && tar cf - include | tar xf - -C $(PREFIX)
	@mkdir -p $(PREFIX)/src && cp -a $(_WRKSRC)/src $(PREFIX)/src/gtest
	@find $(PREFIX) ! -type d > $(_WRKSRC).after.list
	@diff -u $(_WRKSRC).before.list $(_WRKSRC).after.list | \
	grep -v '^\++'|awk -F $(PREFIX)/ '/^\+/{print $$2}' > $(PREFIX)/var/pkg/$(_NAME)
	@echo "done"
do-uninstall:
	@cd $(PREFIX) && cat var/pkg/$(_NAME) | xargs rm -f
	@rm -f $(PREFIX)/var/pkg/$(_NAME)