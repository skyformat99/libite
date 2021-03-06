ACLOCAL_AMFLAGS     = -I m4

doc_DATA            = README.md LICENSE
EXTRA_DIST          = README.md LICENSE ChangeLog.md
pkgconfigdir        = $(libdir)/pkgconfig
pkgconfig_DATA      = libite.pc
SUBDIRS             = src tests

## Generate .deb package
package:
	@dpkg-buildpackage -uc -us -B

## Generate MD5 checksum file
MD5 = md5sum
md5-dist:
	@for file in $(DIST_ARCHIVES); do							\
		$(MD5) $$file > $$file.md5;							\
	done

## Check if tagged in git
release-hook:
	if [ ! `git tag | grep $(PACKAGE_VERSION)` ]; then					\
		echo;										\
		printf "\033[1m\033[41mCannot find release tag $(PACKAGE_VERSION)\033[0m\n";	\
		printf "\033[1m\033[5mDo release anyway?\033[0m "; read yorn;			\
		if [ "$$yorn" != "y" -a "$$yorn" != "Y" ]; then					\
			printf "OK, aborting release.\n";					\
			exit 1;									\
		fi;										\
		echo;										\
	else											\
		echo;										\
		printf "\033[1m\033[42mFound GIT release tag $(PACKAGE_VERSION)\033[0m\n";	\
		printf "\033[1m\033[44m>>Remember to push tags!\033[0m\n";			\
		echo;										\
	fi

## Target to run when building a release
release: distcheck release-hook md5-dist
	@for file in $(DIST_ARCHIVES); do							\
		printf "$$file    \tDistribution tarball\n";					\
		printf "$$file.md5\t"; cat $$file.md5 | cut -f1 -d' ';				\
		mv $$file* ../;									\
	done
