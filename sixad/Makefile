# You know, there are pre-compile DEBs of this...

CXX ?= g++
CXXFLAGS ?= -O2 -Wall
LDFLAGS += -Wl,-Bsymbolic-functions

all: sixad_bins

BINDIR ?= bins
INSTALLDIR ?= 

sixad_bins:
	mkdir -p $(BINDIR)
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-bin.cpp bluetooth.cpp shared.cpp textfile.cpp -o $(BINDIR)/sixad-bin `pkg-config --cflags --libs bluez` -lpthread -fpermissive
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-sixaxis.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o $(BINDIR)/sixad-sixaxis -lpthread -lrt
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-remote.cpp remote.cpp shared.cpp uinput.cpp textfile.cpp -o $(BINDIR)/sixad-remote -lrt
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-raw.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o $(BINDIR)/sixad-raw
	$(CXX) $(CXXFLAGS) $(LDFLAGS) sixad-3in1.cpp sixaxis.cpp shared.cpp uinput.cpp textfile.cpp -o $(BINDIR)/sixad-3in1

clean:
	rm -f *~ $(BINDIR)/*

install:
	install -d $(DESTDIR)/etc/default/
	install -d $(DESTDIR)/etc/init.d/
	install -d $(DESTDIR)/etc/logrotate.d/
	install -d $(DESTDIR)/usr/bin/
	install -d $(DESTDIR)/usr/sbin/
	install -d $(DESTDIR)/var/lib/sixad/
	install -d $(DESTDIR)/var/lib/sixad/profiles/
	install -d $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 $(BINDIR)/sixad-bin $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 $(BINDIR)/sixad-sixaxis $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 $(BINDIR)/sixad-remote $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 $(BINDIR)/sixad-3in1 $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 $(BINDIR)/sixad-raw $(DESTDIR)/usr/sixad/$(INSTALLDIR)/
	install -m 755 sixad-dbus-blocker $(DESTDIR)/usr/sbin/

ifeq "$(wildcard /usr/bin/python2 )" ""
else
	# Python2 force
	sed -i "s/env python/env python2/" $(DESTDIR)/usr/sbin/sixad-dbus-blocker
endif

	@chmod 777 -R $(DESTDIR)/var/lib/sixad/
	@echo "Installation is Complete!"

uninstall:
	rm -f $(DESTDIR)/etc/default/sixad
	rm -f $(DESTDIR)/etc/init.d/sixad
	rm -f $(DESTDIR)/etc/logrotate.d/sixad
	rm -f $(DESTDIR)/usr/bin/sixad
	rm -f $(DESTDIR)/usr/sbin/sixad-bin
	rm -f $(DESTDIR)/usr/sbin/sixad-sixaxis
	rm -f $(DESTDIR)/usr/sbin/sixad-remote
	rm -f $(DESTDIR)/usr/sbin/sixad-raw
	rm -f $(DESTDIR)/usr/sbin/sixad-dbus-blocker
	rm -rf $(DESTDIR)/var/lib/sixad/
