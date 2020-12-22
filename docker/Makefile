build:
	cd ubuntu; make build
	cd ubuntu-x11; make build
	cd ubuntu-x11-i386; make build
	cd pcsx2; make build
	cd rpcs3; make build

push:
	cd ubuntu; make push
	cd ubuntu-x11; make push
	cd ubuntu-x11-i386; make push
	cd pcsx2; make push
	cd rpcs3; make push

release:
	cd ubuntu; make build && make push
	cd ubuntu-x11; make build && make push
	cd ubuntu-x11-i386; make build && make push
	cd pcsx2; make build && make push
	cd rpcs3; make build && make push

docs:
	cd ubuntu; make docs
	cd ubuntu-x11; make docs
	cd ubuntu-x11-i386; make docs
	cd pcsx2; make docs
	cd rpcs3; make docs

clean:
	cd ubuntu; make clean
	cd ubuntu-x11; make clean
	cd ubuntu-x11-i386; make clean
	cd pcsx2; make clean
	cd rpcs3; make clean
