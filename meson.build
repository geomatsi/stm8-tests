project('stm8 tests', 'c',
	version : '0.1',
)

stm8cc = find_program('sdcc', required : true)
stm8flash = find_program('stm8flash', required : true)

stm8cc_args = ['-mstm8', '--Werror', '--std-c99']
stm8ld_args = ['-mstm8', '--out-fmt-ihx']
stm8cc_incs = ['-I' , '../include']

progs = [
	['blink.ihx', ['src/blink.c', 'src/delay.c'], 'stm8s103f3', 'GPIO/LED example'],
]

t_flash = 'flash_@0@'

foreach p : progs

	gen = generator(stm8cc,
		output : '@BASENAME@.rel',
		arguments : stm8cc_args + stm8cc_incs + ['-c', '@INPUT@'] + ['-o', '@OUTPUT@'],
	)

	obj = gen.process(p[1])

	exe = custom_target(p[0],
		input : obj,
		output : p[0],
		command : [stm8cc, stm8ld_args, '@INPUT@', '-o', '@OUTPUT@'],
	)

	run = run_target(t_flash.format(p[0]),
		command : [stm8flash, '-s', 'flash', '-c', 'stlinkv2', '-p', p[2], '-w', '@0@'.format(exe.full_path())],
		depends : exe,
	)

	message('@0@ (@1@) targets: @0@ @2@'.format(p[0], p[3], t_flash.format(p[0])))
endforeach
