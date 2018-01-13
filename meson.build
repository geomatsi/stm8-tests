project('stm8 tests', 'c',
	version : '0.1',
)

#assert(meson.is_cross_build(), 'Use sdcc for stm8 target')

stm8cc = find_program('sdcc', required : true)
stm8flash = find_program('stm8flash', required : true)

stm8cc_args = ['-mstm8', '--Werror', '--std-c99', '--out-fmt-ihx', '-I' , '../include']

progs = [
	['blink', ['src/blink.c'], 'blink.ihx', 'stm8s103f3', 'GPIO/LED example'],
]

t_flash = 'flash_@0@'

foreach p : progs

	e = custom_target(p[0],
		input : p[1],
		output : p[2],
		command : [stm8cc, stm8cc_args, '@INPUT@'],
	)

	r = run_target(t_flash.format(p[0]),
		command : [stm8flash, '-s', 'flash', '-c', 'stlinkv2', '-p', p[3], '-w', '@0@'.format(e.full_path())],
		depends : e,
	)

	message('@0@ (@1@) targets: @0@ @2@'.format(p[0], p[4], t_flash.format(p[0])))
endforeach
