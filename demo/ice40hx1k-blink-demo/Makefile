PROJ = blink
PIN_DEF = ice40hx1k-evb.pcf
DEVICE = hx1k
FLASHROM = /usr/bin/flashrom
ARDUINO = /usr/bin/iceprogduino

all: sim sint pack vvp

sim: $(PROJ)_tb.vcd

sint: $(PROJ).bin $(PROJ).asc $(PROJ).json $(PROJ).rpt $(PROJ)_tb.out

%_tb.out: %.v
	iverilog -o $@ $<

%.json: %.v
	yosys -p 'synth_ice40 -top top -json $@' $<

%.asc: %.json
	nextpnr-ice40 --hx1k --package vq100 --json $< --pcf $(PIN_DEF) --asc $@

%.bin: %.asc
	icepack $< $@

pack: $(PROJ).img

%.img: %.bin
	dd if=/dev/zero bs=2M count=1 | tr '\0' '\377' > $@
	dd if=$< conv=notrunc of=$@

buspirate: $(PROJ).img
	@echo 'Executing flashrom'
	@echo 'hint: Install from archive.'
	@echo '---------------------------'
	$(FLASHROM) -p buspirate_spi:dev=/dev/ttyUSB0,spispeed=2M -w $<


%_tb.vcd: %_tb.out
	@echo "No testbench added"
	@echo "------------------"
	vvp -vv $<

showsim: sim
	gtkwave $(PROJ)_tb.out &

%.rpt: %.asc
	icetime -d $(DEVICE) -mtr $@ $<

prog: $(PROJ).bin
	@echo 'Executing iceprogduino'
	@echo 'hint: <repo>/programmer/iceprogduino'
	@echo '------------------------------------'
	$(ARDUINO) $<

sudo-prog: $(PROJ).bin
	@echo 'Executing iceprogduino as root.'
	@echo 'hint: <repo>/programmer/iceprogduino'
	@echo '------------------------------------'
	sudo $(ARDUINO) $<

clean:
	@echo "Cleaning directory..."
	@rm -f \
		$(PROJ).asc \
		$(PROJ).bin \
		$(PROJ).json \
		$(PROJ).bin \
		$(PROJ).img \
		$(PROJ).rpt \
		*_tb*

.PHONY: all buspirate prog sudo-prog vvp showsim clean
