# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)

.PHONY: all

all: clean init
	mkdir --parents $(PWD)/build/Boilerplate.AppDir	
	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
	    libgdk-pixbuf2.0-0 libatk1.0-0 libatk-bridge2.0-0 libgtk2.0-0 \
	    libreadline8 at-spi2-core libpango-1.0-0 libselinux1

	cp /tmp/apprepo/brave-browser*.deb $(PWD)/build/build.deb
	dpkg -x $(PWD)/build/build.deb $(PWD)/build
	cp -r $(PWD)/build/opt/brave*/brave $(PWD)/build/Boilerplate.AppDir

	echo "LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}:\$${APPDIR}/brave"  >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo "export LD_LIBRARY_PATH=\$${LD_LIBRARY_PATH}"             >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                        >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                                        >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo 'exec $${APPDIR}/brave/brave "$${@}"'                     >> $(PWD)/build/Boilerplate.AppDir/AppRun

	rm --force $(PWD)/build/Boilerplate.AppDir/*.svg 		|| true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.desktop    || true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.png 		|| true

	cp --force $(PWD)/AppDir/*.png 		$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.desktop 	$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.svg 		$(PWD)/build/Boilerplate.AppDir/ || true

	export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/Brave.AppImage
	chmod +x $(PWD)/Brave.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://apprepo.de/appimage/download/apprepo \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/13/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
