1.Files
	OSD10_rtl_no_df is source folder of 264 decoder, no deblocking filter verison,
	for verison with deblocking filter, please contact author(eebq, qq:1517642772)
	OSD10_testbench is testbench folder of 264 decoder.
	software is ralated software:1.C Model 2.pli_fputc 3.hex2bin_new
2. how to simulate
	1.in modelsim, create a project.
	2.add all files in 3 directories:
	 	"OSD10_testbench", "OSD10_rtl_no_df/display", "OSD10_rtl_no_df/ip_core"
	3.complile all files,
	4.there are two modes of simulation output , one is binary mode and the other is
		ascii mode, the default mode is binary mode.
		1.binary output mode:
			in modelsim console window, type 
			"vsim -vopt -pli pli_fputc64.dll work.bitstream_tb".
			pli_fputc64.dll is a pli library I write for dump binary out.yuv file, it runs
			in windows 64bit,
			if you are using windows 32 bit, instead it with fputc32.dll. 
			the source of pli_fputc is in software/pli_fputc_src directory.
		2.ascii output mode: 
			you can simulate without pli_fputc.dll, it simulate much
			faster using command "vsim -vopt work.bitstream_tb". 
			to do this, you should modify Line 378 to 
			"ext_ram_32 #(.BinMode(0)) ext_ram_32".
			In this mode, you must convert ascii file "out.yuv" to binary
			mode with hex2bin_new.exe.
			the source of hex2bin_new is in software/hex2bin_new_src directory.

3. C model
	1.About C model
	-- A simple C model is provided to compare the output of HDL and software,
	--C model is in software/c_model/src/
	-- The C model is tested, the output yuv file is same with JM86.
	-- by default the deblocking filter is turned off, to open it,just
	   uncomment line 363:"deblocking_filter(slice_header, pps);"
	2.how to build and run
		1.cd to this directory.
		2.type "make" to build bitstream.exe, gcc is required to build it.
		3.run bitstream.exe to decode the "in.264" file in the same directory.
