###############################################################################
#                                                                             #
# Copyright 2016 myStorm Copyright and related                                #
# rights are licensed under the Solderpad Hardware License, Version 0.51      #
# (the “License”); you may not use this file except in compliance with        #
# the License. You may obtain a copy of the License at                        #
# http://solderpad.org/licenses/SHL-0.51. Unless required by applicable       #
# law or agreed to in writing, software, hardware and materials               #
# distributed under this License is distributed on an “AS IS” BASIS,          #
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or             #
# implied. See the License for the specific language governing                #
# permissions and limitations under the License.                              #
#                                                                             #
###############################################################################

# Default name of serial adaptor if none is provided by the user
#SERIAL := /dev/cu.usbmodem1421 # For macOS (maybe)
SERIAL := /dev/ttyACM0 # The default for linux (usually)

# Leave as the basic make, so that people can initially get something running
chip.bin: ./chip.v ./blink.v blackice.pcf
		yosys -q -p "synth_ice40 -blif chip.blif" ./chip.v ./blink.v
		arachne-pnr -d 8k -P tq144:4k -p blackice.pcf chip.blif -o chip.txt
		icepack chip.txt chip.bin

# .PHONY: blink
# blink: ./chip.v ./blink.v blackice.pcf
# 		yosys -q -p "synth_ice40 -blif chip.blif" ./chip.v ./blink.v
# 		arachne-pnr -d 8k -P tq144:4k -p blackice.pcf chip.blif -o chip.txt
# 		icepack chip.txt chip.bin

.PHONY: upload-linux
upload-linux:
	cat $(SERIAL) > /dev/null 2>&1 &
	stty -F $(SERIAL) raw
	cat chip.bin > $(SERIAL)
	ps x | grep "cat $(SERIAL)" | head -1 | \
	       sed -e 's/\([[:digit:]]*\) .*$$/now run "kill \1"/'

# Upload to a Mac, may have to change name of the port accordingly

.PHONY: upload-mac
upload-mac:
	cat $(SERIAL) > /dev/null 2>&1 &
	stty -f $(SERIAL) raw ;\
	cat chip.bin > $(SERIAL) ;\
	ps x | grep "cat $(SERIAL)" | head -1 | \
	       sed -e 's/\([[:digit:]]*\) .*$$/now run "kill \1"/'

.PHONY: clean
clean:
	$(RM) -f chip.blif chip.txt chip.ex chip.bin

#Based of the oringinal makefile from the blackice tutorial (gitlab.com/Folknology/mystorm/tree/BlackIce/tutorial/BlackIce), with additions by Dan Gorringe
